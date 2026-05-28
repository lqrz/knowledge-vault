# Give Claude Custom Tools

> Source: https://code.claude.com/docs/en/agent-sdk/custom-tools

Custom tools extend the Agent SDK by letting you define your own functions that Claude can call during a conversation. Using the SDK's in-process MCP server, you can give Claude access to databases, external APIs, domain-specific logic, or any other capability your application needs.

## Quick Reference

| If you want to...                            | Do this                                                                 |
| :------------------------------------------- | :---------------------------------------------------------------------- |
| Define a tool                                | Use `@tool` (Python) or `tool()` (TypeScript) with name, description, schema, handler |
| Register a tool with Claude                  | Wrap in `create_sdk_mcp_server` / `createSdkMcpServer`, pass to `mcpServers` in `query()` |
| Pre-approve a tool                           | Add to `allowed_tools` / `allowedTools`                                 |
| Remove a built-in tool                       | Pass a `tools` array listing only the built-ins you want                |
| Let Claude call tools in parallel            | Set `readOnlyHint: true` on tools with no side effects                  |
| Handle errors without stopping the loop      | Return `isError: true` instead of throwing                              |
| Return images or files                       | Use `image` or `resource` blocks in the content array                   |
| Return machine-readable JSON                 | Set `structuredContent` on the result                                   |
| Scale to many tools                          | Use tool search to load tools on demand                                 |

## Create a Custom Tool

A tool is defined by four parts:

- **Name:** unique identifier Claude uses to call the tool
- **Description:** what the tool does — Claude reads this to decide when to call it
- **Input schema:** arguments Claude must provide. TypeScript uses Zod; Python uses a dict like `{"latitude": float}` or a full JSON Schema dict for enums/nested objects
- **Handler:** async function that runs when Claude calls the tool, receiving validated arguments and returning `{content, structuredContent?, isError?}`

After defining a tool, wrap it in `createSdkMcpServer` (TS) / `create_sdk_mcp_server` (Python). The server runs in-process, not as a separate process.

### Weather Tool Example

```python
from typing import Any
import httpx
from claude_agent_sdk import tool, create_sdk_mcp_server

@tool(
    "get_temperature",
    "Get the current temperature at a location",
    {"latitude": float, "longitude": float},
)
async def get_temperature(args: dict[str, Any]) -> dict[str, Any]:
    async with httpx.AsyncClient() as client:
        response = await client.get(
            "https://api.open-meteo.com/v1/forecast",
            params={
                "latitude": args["latitude"],
                "longitude": args["longitude"],
                "current": "temperature_2m",
                "temperature_unit": "fahrenheit",
            },
        )
        data = response.json()
    return {
        "content": [{"type": "text", "text": f"Temperature: {data['current']['temperature_2m']}°F"}]
    }

weather_server = create_sdk_mcp_server(name="weather", version="1.0.0", tools=[get_temperature])
```

```typescript
import { tool, createSdkMcpServer } from "@anthropic-ai/claude-agent-sdk";
import { z } from "zod";

const getTemperature = tool(
  "get_temperature",
  "Get the current temperature at a location",
  {
    latitude: z.number().describe("Latitude coordinate"),
    longitude: z.number().describe("Longitude coordinate")
  },
  async (args) => {
    const response = await fetch(
      `https://api.open-meteo.com/v1/forecast?latitude=${args.latitude}&longitude=${args.longitude}&current=temperature_2m&temperature_unit=fahrenheit`
    );
    const data: any = await response.json();
    return {
      content: [{ type: "text", text: `Temperature: ${data.current.temperature_2m}°F` }]
    };
  }
);

const weatherServer = createSdkMcpServer({ name: "weather", version: "1.0.0", tools: [getTemperature] });
```

### Call a Custom Tool

Pass the MCP server to `query` via `mcpServers`. The key becomes the `{server_name}` in each tool's fully qualified name: `mcp__{server_name}__{tool_name}`.

```python
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage

async def main():
    options = ClaudeAgentOptions(
        mcp_servers={"weather": weather_server},
        allowed_tools=["mcp__weather__get_temperature"],
    )
    async for message in query(prompt="What's the temperature in San Francisco?", options=options):
        if isinstance(message, ResultMessage) and message.subtype == "success":
            print(message.result)

asyncio.run(main())
```

```typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

for await (const message of query({
  prompt: "What's the temperature in San Francisco?",
  options: {
    mcpServers: { weather: weatherServer },
    allowedTools: ["mcp__weather__get_temperature"]
  }
})) {
  if (message.type === "result" && message.subtype === "success") {
    console.log(message.result);
  }
}
```

### Add More Tools

A server holds as many tools as you list in its `tools` array. Use a wildcard `mcp__weather__*` to cover every tool the server exposes.

Making parameters optional:
- TypeScript: add `.default()` to the Zod field
- Python: omit from schema, mention in description string, read with `args.get()`

Every tool in the array consumes context window space on every turn. For dozens of tools, use tool search to defer loading.

### Tool Annotations

Pass as fifth argument to `tool()` (TypeScript) or via `annotations=` keyword (`@tool` in Python). All fields are Booleans.

| Field             | Default | Meaning                                                                                      |
| :---------------- | :------ | :------------------------------------------------------------------------------------------- |
| `readOnlyHint`    | `false` | Tool does not modify environment. Enables parallel calls with other read-only tools.         |
| `destructiveHint` | `true`  | Tool may perform destructive updates. Informational only.                                    |
| `idempotentHint`  | `false` | Repeated calls with same args have no additional effect. Informational only.                 |
| `openWorldHint`   | `true`  | Tool reaches systems outside your process. Informational only.                               |

Annotations are metadata, not enforcement — the handler does what it does regardless.

## Control Tool Access

### Tool Name Format

- Pattern: `mcp__{server_name}__{tool_name}`
- Example: `get_temperature` in server `weather` → `mcp__weather__get_temperature`

### Configure Allowed Tools

Two layers: **availability** (whether tool appears in Claude's context) and **permission** (whether a call is approved once Claude attempts it).

| Option                    | Layer        | Effect                                                                                 |
| :------------------------ | :----------- | :--------------------------------------------------------------------------------------|
| `tools: ["Read", "Grep"]` | Availability | Only listed built-ins in Claude's context. MCP tools unaffected.                      |
| `tools: []`               | Availability | All built-ins removed. Claude can only use MCP tools.                                  |
| `allowedTools`            | Permission   | Listed tools run without prompt. Unlisted go through permission flow.                  |
| `disallowedTools` (bare)  | Availability | Bare name like `"Bash"` removes from context entirely.                                 |
| `disallowedTools` (scoped)| Permission   | Scoped rule like `"Bash(rm *)"` leaves in context but denies matching calls.          |

## Handle Errors

| What happens                             | Result                                                                              |
| :--------------------------------------- | :---------------------------------------------------------------------------------- |
| Handler throws uncaught exception        | Agent loop stops. Claude never sees the error. `query()` call fails.               |
| Handler returns `isError: true`          | Agent loop continues. Claude sees error as data and can retry or explain failure.   |

```python
@tool("fetch_data", "Fetch data from an API", {"endpoint": str})
async def fetch_data(args: dict[str, Any]) -> dict[str, Any]:
    try:
        async with httpx.AsyncClient() as client:
            response = await client.get(args["endpoint"])
            if response.status_code != 200:
                return {
                    "content": [{"type": "text", "text": f"API error: {response.status_code}"}],
                    "is_error": True,
                }
            return {"content": [{"type": "text", "text": json.dumps(response.json(), indent=2)}]}
    except Exception as e:
        return {
            "content": [{"type": "text", "text": f"Failed to fetch data: {str(e)}"}],
            "is_error": True,
        }
```

## Return Images and Resources

The `content` array accepts `text`, `image`, and `resource` blocks, mixable in the same response.

### Images

Image blocks carry bytes inline as base64. No URL field — fetch URL content in the handler, then base64-encode.

| Field      | Type      | Notes                                                              |
| :--------- | :-------- | :----------------------------------------------------------------- |
| `type`     | `"image"` |                                                                    |
| `data`     | `string`  | Raw base64 bytes — no `data:image/...;base64,` prefix              |
| `mimeType` | `string`  | Required: `image/png`, `image/jpeg`, `image/webp`, `image/gif`     |

```python
@tool("fetch_image", "Fetch an image from a URL", {"url": str})
async def fetch_image(args):
    async with httpx.AsyncClient() as client:
        response = await client.get(args["url"])
    return {
        "content": [{
            "type": "image",
            "data": base64.b64encode(response.content).decode("ascii"),
            "mimeType": response.headers.get("content-type", "image/png"),
        }]
    }
```

### Resources

A resource block embeds content identified by a URI. The URI is a label; actual content rides in `text` or `blob`.

| Field               | Type         | Notes                                              |
| :------------------ | :----------- | :------------------------------------------------- |
| `type`              | `"resource"` |                                                    |
| `resource.uri`      | `string`     | Identifier. Any URI scheme.                        |
| `resource.text`     | `string`     | Content if text. Provide this or `blob`, not both. |
| `resource.blob`     | `string`     | Content base64-encoded if binary.                  |
| `resource.mimeType` | `string`     | Optional.                                          |

```python
return {
    "content": [{
        "type": "resource",
        "resource": {
            "uri": "file:///tmp/report.md",  # label, not a path the SDK reads
            "mimeType": "text/markdown",
            "text": "# Report\n...",
        },
    }]
}
```

## Return Structured Data

`structuredContent` is an optional JSON object separate from `content`. Use it for raw values Claude can read as exact fields rather than parsing from text.

When set, Claude receives the JSON plus any image/resource blocks. Text blocks are not forwarded (assumed to duplicate the structured data).

```typescript
return {
  content: [{ type: "image", data: chartPngBuffer.toString("base64"), mimeType: "image/png" }],
  structuredContent: {
    series: "temperature_2m",
    unit: "fahrenheit",
    points: [62.1, 63.4, 65.0, 64.2]
  }
};
```

> **Note:** The Python `@tool` decorator forwards only `content` and `is_error`. To return `structuredContent` from Python, use a standalone MCP server instead.

## Example: Unit Converter

Demonstrates enum schemas and unsupported-input error handling.

- TypeScript: use `z.enum()`
- Python: dict schema has no enum support, so pass full JSON Schema dict

```python
@tool(
    "convert_units",
    "Convert a value from one unit to another",
    {
        "type": "object",
        "properties": {
            "unit_type": {"type": "string", "enum": ["length", "temperature", "weight"]},
            "from_unit": {"type": "string"},
            "to_unit": {"type": "string"},
            "value": {"type": "number"},
        },
        "required": ["unit_type", "from_unit", "to_unit", "value"],
    },
)
async def convert_units(args):
    conversions = {
        "length": {"kilometers_to_miles": lambda v: v * 0.621371, ...},
        "temperature": {"celsius_to_fahrenheit": lambda v: (v * 9) / 5 + 32, ...},
        "weight": {"kilograms_to_pounds": lambda v: v * 2.20462, ...},
    }
    key = f"{args['from_unit']}_to_{args['to_unit']}"
    fn = conversions.get(args["unit_type"], {}).get(key)
    if not fn:
        return {
            "content": [{"type": "text", "text": f"Unsupported: {args['from_unit']} to {args['to_unit']}"}],
            "is_error": True,
        }
    result = fn(args["value"])
    return {"content": [{"type": "text", "text": f"{args['value']} {args['from_unit']} = {result:.4f} {args['to_unit']}"}]}
```
