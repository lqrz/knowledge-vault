# Returning Content from Tools

A tool result's `content` array can hold three block types: `text`, `image`, and `resource`. You can mix them in the same response.

## Text Blocks

The simplest block type. Claude reads the text as the tool's output.

```python
return {"content": [{"type": "text", "text": "72°F"}]}
```

## Image Blocks

Images are carried inline as base64-encoded bytes. There is no URL field — if your image lives at a URL, fetch the bytes in the handler first.

| Field      | Type      | Notes                                                               |
| :--------- | :-------- | :------------------------------------------------------------------ |
| `type`     | `"image"` |                                                                     |
| `data`     | `string`  | Raw base64 — no `data:image/...;base64,` prefix                     |
| `mimeType` | `string`  | Required. `image/png`, `image/jpeg`, `image/webp`, `image/gif`      |

```python
import base64
import httpx

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

```typescript
tool("fetch_image", "Fetch an image from a URL", { url: z.string().url() }, async (args) => {
  const response = await fetch(args.url);
  const buffer = Buffer.from(await response.arrayBuffer());
  return {
    content: [{
      type: "image",
      data: buffer.toString("base64"),
      mimeType: response.headers.get("content-type") ?? "image/png"
    }]
  };
});
```

Claude processes image blocks as visual input — it can describe, analyze, or reason about the image content.

## Resource Blocks

A resource block embeds named content identified by a URI. Use this when the result has an identity (a generated file, a record from an external system) that Claude may want to reference later.

| Field               | Type         | Notes                                               |
| :------------------ | :----------- | :-------------------------------------------------- |
| `type`              | `"resource"` |                                                     |
| `resource.uri`      | `string`     | Any URI scheme. Label only — the SDK does not read this path. |
| `resource.text`     | `string`     | The content, if text. Provide this or `blob`.       |
| `resource.blob`     | `string`     | The content base64-encoded, if binary.              |
| `resource.mimeType` | `string`     | Optional.                                           |

```python
return {
    "content": [{
        "type": "resource",
        "resource": {
            "uri": "file:///tmp/report.md",   # label, not a path the SDK reads
            "mimeType": "text/markdown",
            "text": "# Report\n...",           # actual content inline
        },
    }]
}
```

## Structured Content (JSON Results)

`structuredContent` is an optional JSON object on the result, separate from `content`. It returns raw values Claude can read as exact fields rather than parsing them out of text.

When `structuredContent` is set:
- Claude receives the JSON plus any image/resource blocks from `content`
- Text blocks in `content` are **not** forwarded (assumed to duplicate the structured data)

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

> **Python limitation:** The `@tool` decorator forwards only `content` and `is_error`. To return `structuredContent` from Python, use a standalone MCP server instead of an in-process SDK server.

## Mixing Block Types

You can return multiple blocks in a single result — for example, a text summary alongside an image, or a resource alongside an explanatory text block:

```python
return {
    "content": [
        {"type": "text", "text": "Here is the chart you requested:"},
        {"type": "image", "data": base64_png, "mimeType": "image/png"},
    ]
}
```

## Related

- [[defining-tools]]
- [[error-handling]]

## Sources

- [[../../raw/custom-tools/index|Agent SDK: Give Claude Custom Tools]] — Anthropic, 2025.
