---
id: mcp-tool-search-metadata
url: metadata/mcp/tools-reference/search-metadata
title: search_metadata
weight: 2
description: "The search_metadata MCP tool returns only the metadata properties matching a category, name, or value filter — the targeted alternative to reading everything."
keywords: search_metadata MCP tool, find metadata property, filter document properties AI, metadata search agent
productName: GroupDocs.Metadata MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

`search_metadata` returns **only the matching properties**, filtered by `category`, `nameContains`, or `valueContains`. Use it when the question is about a specific property rather than the whole set. Example prompt: *"Does this file mention the old company name anywhere in its properties?"*

**Tool description (as the AI agent sees it):**

> Searches the metadata inside a single document and returns only the matching properties as JSON — use this instead of ReadMetadata when the user asks about a SPECIFIC property rather than the whole set. Call it for 'show me the author of report.pdf', 'what is the creation date?', 'does photo.jpg have GPS?', or to check a value like 'does contract.docx have Author = ABC?'. Filters (combine any): category (person, content, time, tool, legal, corporate, document, gps, comments, keywords), nameContains, valueContains — all case-insensitive. Supports PDF, DOCX, XLSX, PPTX, JPEG, PNG, TIFF, MP3, MP4 and 100+ more formats. Returns a JSON object with fields `count` and `properties` (array of { name, value, category }); `count` 0 means no match. On failure the response text starts with 'Metadata search failed for'.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "metadata/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `category` | string | no | Optional category filter: person, content, time, tool, legal, corporate, document, gps, comments, keywords |
| `nameContains` | string | no | Optional: only properties whose name contains this text (case-insensitive) |
| `valueContains` | string | no | Optional: only properties whose value contains this text (case-insensitive) |
| `password` | string | no | Password for protected documents |

## Example call

```json
{
  "name": "search_metadata",
  "arguments": {
    "file": {
      "filePath": "report.pdf"
    },
    "nameContains": "author"
  }
}
```

## Result

A JSON array of the properties that matched, with their package, name, and value.

The `valueContains` filter is the interesting one for privacy work: it answers *"does any property still contain this name / this path / this address?"* across every metadata package in the file, in one call.

On failure the text starts with `Metadata search failed for`, followed by the exception type and message.

## Example prompts

* *"Does this document still mention the old company name in its properties?"*
* *"Show me just the author and last-modified-by fields."*
* *"Find any property whose value contains an @ sign."*
