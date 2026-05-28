# Defining Custom Tools

Custom tools let you expose any Python or TypeScript function to Claude during an agent session. The SDK's in-process MCP server bridges your function and Claude's tool-calling interface without spawning a separate process.

## The Four Parts of a Tool

Every tool has four required pieces:

**Name** — the unique identifier Claude uses when calling the tool.

**Description** — a natural-language explanation of what the tool does. Claude reads this at runtime to decide when to call it. Good descriptions are specific about inputs, outputs, and side effects.

**Input schema** — the arguments Claude must provide. TypeScript uses Zod schemas (giving you automatic type inference in the handler). Python accepts either a shorthand dict like `{"latitude": float, "longitude": float}` or a full JSON Schema dict when you need enums, ranges, optional fields, or nested objects.

**Handler** — an async function that receives the validated arguments and returns a result object with:
- `content` (required): array of `text`, `image`, or `resource` blocks
- `structuredContent` (optional): machine-readable JSON alongside the content
- `isError` / `is_error` (optional): `true` signals a failed call so Claude can react

## Python: `@tool` Decorator

```python
from claude_agent_sdk import tool, create_sdk_mcp_server

@tool(
    "get_temperature",
    "Get the current temperature at a location",
    {"latitude": float, "longitude": float},
)
async def get_temperature(args):
    # args["latitude"], args["longitude"] are validated floats
    return {"content": [{"type": "text", "text": "72°F"}]}
```

For optional parameters, omit the key from the schema dict, mention it in the description, and use `args.get("key", default)` in the handler. The `@tool` decorator does not support `.default()` — that's a Zod concept.

For enums or nested objects, pass a full JSON Schema dict as the third argument:

```python
@tool(
    "convert_units",
    "Convert between units",
    {
        "type": "object",
        "properties": {
            "unit_type": {"type": "string", "enum": ["length", "temperature", "weight"]},
            "value": {"type": "number"},
        },
        "required": ["unit_type", "value"],
    },
)
async def convert_units(args): ...
```

## TypeScript: `tool()` Helper

```typescript
import { tool } from "@anthropic-ai/claude-agent-sdk";
import { z } from "zod";

const getTemperature = tool(
  "get_temperature",
  "Get the current temperature at a location",
  {
    latitude: z.number().describe("Latitude coordinate"),
    longitude: z.number().describe("Longitude coordinate"),
    units: z.enum(["fahrenheit", "celsius"]).default("fahrenheit") // .default() makes optional
  },
  async (args) => {
    // args is typed: { latitude: number; longitude: number; units: "fahrenheit" | "celsius" }
    return { content: [{ type: "text", text: `72°F` }] };
  }
);
```

Use `.describe()` on Zod fields to add per-field descriptions Claude sees. Use `.default()` to make a parameter optional.

## Registering Tools

Wrap tools in a server and pass it to `query()`:

```python
weather_server = create_sdk_mcp_server(name="weather", version="1.0.0", tools=[get_temperature])

options = ClaudeAgentOptions(
    mcp_servers={"weather": weather_server},
    allowed_tools=["mcp__weather__get_temperature"],  # pre-approve so no prompt
)
```

The key in `mcp_servers` becomes `{server_name}` in the fully qualified tool name: `mcp__{server_name}__{tool_name}`. Use a wildcard `mcp__weather__*` to pre-approve all tools on a server at once.

## Adding Annotations

Annotations are optional metadata about a tool's behavior. Pass as `annotations=` keyword (Python) or as the fifth argument (TypeScript).

```python
from claude_agent_sdk import ToolAnnotations

@tool(
    "get_temperature", "...", {"latitude": float, "longitude": float},
    annotations=ToolAnnotations(readOnlyHint=True),
)
async def get_temperature(args): ...
```

| Field             | Default | Effect                                                             |
| :---------------- | :------ | :----------------------------------------------------------------- |
| `readOnlyHint`    | `false` | Enables parallel execution with other read-only tools              |
| `destructiveHint` | `true`  | Informational: tool may perform destructive updates                |
| `idempotentHint`  | `false` | Informational: repeated calls with same args have no extra effect  |
| `openWorldHint`   | `true`  | Informational: tool reaches external systems                       |

Annotations describe intent — they are not enforced. A `readOnlyHint: true` tool can still write to disk.

## Context Window Costs

Every tool in the server's `tools` array occupies context window space on every turn. If you're defining more than a handful of tools, consider [[tool-search]] to defer loading until Claude needs them.

## Related

- [[tool-access-control]]
- [[error-handling]]
- [[returning-content]]

## Sources

- [[../../raw/custom-tools/index|Agent SDK: Give Claude Custom Tools]] — Anthropic, 2025.
