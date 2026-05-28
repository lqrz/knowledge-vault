# Error Handling in Custom Tools

How a handler reports errors determines whether the agent loop continues or stops. The key distinction is between throwing an exception and returning an error result.

## Throwing vs. Returning

| What happens                        | Result                                                                            |
| :---------------------------------- | :-------------------------------------------------------------------------------- |
| Handler throws uncaught exception   | Agent loop stops. Claude never sees the error. The `query()` call fails entirely. |
| Handler returns `isError: true`     | Agent loop continues. Claude sees the error as data and can retry or explain it.  |

Always catch errors inside the handler and return them as error results. The only case where you'd let an exception propagate is if you *want* the agent to stop (e.g., an unrecoverable infrastructure failure).

## Pattern: Catch and Return

```python
import json
import httpx
from typing import Any

@tool("fetch_data", "Fetch data from an API", {"endpoint": str})
async def fetch_data(args: dict[str, Any]) -> dict[str, Any]:
    try:
        async with httpx.AsyncClient() as client:
            response = await client.get(args["endpoint"])
            if response.status_code != 200:
                # Non-200 is a domain error — return it so Claude can react
                return {
                    "content": [{"type": "text", "text": f"API error: {response.status_code} {response.reason_phrase}"}],
                    "is_error": True,
                }
            return {"content": [{"type": "text", "text": json.dumps(response.json(), indent=2)}]}
    except Exception as e:
        # Network failure, invalid JSON, etc. — keep the loop alive
        return {
            "content": [{"type": "text", "text": f"Failed to fetch data: {str(e)}"}],
            "is_error": True,
        }
```

```typescript
tool("fetch_data", "Fetch data from an API", { endpoint: z.string().url() }, async (args) => {
  try {
    const response = await fetch(args.endpoint);
    if (!response.ok) {
      return {
        content: [{ type: "text", text: `API error: ${response.status} ${response.statusText}` }],
        isError: true
      };
    }
    return { content: [{ type: "text", text: JSON.stringify(await response.json(), null, 2) }] };
  } catch (error) {
    return {
      content: [{ type: "text", text: `Failed: ${error instanceof Error ? error.message : String(error)}` }],
      isError: true
    };
  }
});
```

## What Claude Does with an Error Result

Claude receives the `content` array from an `isError: true` result and treats it as data. It can:
- Retry the tool with adjusted arguments
- Try a different tool
- Report the failure to the user with explanation

This is more useful than a crash — Claude can often recover or explain gracefully.

## Unsupported Input Errors

When a tool receives valid input that it simply doesn't support (e.g., an unsupported unit conversion pair), return `isError: true` rather than returning an empty or misleading success result:

```python
fn = conversions.get(args["unit_type"], {}).get(key)
if not fn:
    return {
        "content": [{"type": "text", "text": f"Unsupported conversion: {args['from_unit']} to {args['to_unit']}"}],
        "is_error": True,
    }
```

This lets Claude tell the user exactly what went wrong rather than silently returning nothing.

## Related

- [[defining-tools]]
- [[returning-content]]

## Sources

- [[../../raw/custom-tools/index|Agent SDK: Give Claude Custom Tools]] — Anthropic, 2025.
