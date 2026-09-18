---
id: mcp-tools-reference
url: metadata/mcp/tools-reference
title: Tools reference
weight: 2
description: "Complete reference of every tool the GroupDocs.Metadata MCP server exposes to AI agents, with parameters, example prompts, and results."
keywords: MCP tools list document metadata, read_metadata MCP tool, remove metadata MCP, EXIF XMP MCP tools reference
productName: GroupDocs.Metadata MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

Complete reference of every tool the GroupDocs.Metadata MCP server exposes to AI agents, with parameters, example prompts, and results. Captured from a live `tools/list` call against server version **26.9.0** (raw capture: `tools-list.generated.json` in this section's source).

| Tool | What it does |
|---|---|
| [`read_metadata`]({{< ref "metadata/mcp/tools-reference/read-metadata.md" >}}) | Reads every metadata property from a document and returns them as JSON |
| [`search_metadata`]({{< ref "metadata/mcp/tools-reference/search-metadata.md" >}}) | Returns only the properties matching a category, name, or value filter |
| [`write_metadata`]({{< ref "metadata/mcp/tools-reference/write-metadata.md" >}}) | Sets, changes, or adds a single property and saves the updated file |
| [`remove_metadata`]({{< ref "metadata/mcp/tools-reference/remove-metadata.md" >}}) | Strips all metadata, or only the categories you name |
| [`get_document_info`]({{< ref "metadata/mcp/tools-reference/get-document-info.md" >}}) | Returns file type, size, page count, and encryption status |
| [`get_license_status`]({{< ref "metadata/mcp/tools-reference/get-license-status.md" >}}) | Reports the active licensing mode and, under metered licensing, consumption |

## The FileInput shape

Every tool takes its document through the same `file` object — pass **either** a name from your storage folder **or** inline content:

```json
{ "file": { "filePath": "report.pdf" } }
```

| Field | Type | Description |
|---|---|---|
| `filePath` | string | File path or name in the configured storage folder |
| `fileContent` | string | Base64-encoded file content (alternative to `filePath`) |
| `fileName` | string | Original filename with extension — required with `fileContent`. Since **26.9.0** it also works on its own, resolved from the storage folder exactly like `filePath` |

You rarely write this JSON yourself: the AI agent does, from your plain-language prompt. Missing files are not an error to fear — the tool responds with the list of available files so the agent can correct itself.
