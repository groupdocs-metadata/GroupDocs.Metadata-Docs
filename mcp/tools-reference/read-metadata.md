---
id: mcp-tool-read-metadata
url: metadata/mcp/tools-reference/read-metadata
title: read_metadata
weight: 1
description: "The read_metadata MCP tool reads every metadata property from a document — author, title, dates, custom properties, EXIF and XMP — and returns them as JSON."
keywords: read_metadata MCP tool, read document properties AI, EXIF reader MCP, XMP metadata agent
productName: GroupDocs.Metadata MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

`read_metadata` reads the metadata properties of a document — author, title, creation date, custom properties, and format-specific sets such as EXIF and XMP — and returns them as JSON. Example prompt: *"Who is the author of report.pdf, and when was it created?"*

**Tool description (as the AI agent sees it):**

> Reads all metadata properties from a document (author, title, creation date, custom properties) and returns them as JSON. Call this tool immediately whenever the user asks to read metadata, show document properties, or get author/title/date info. Do NOT pre-check whether files exist — just pass the filename the user provided. The tool resolves files from storage and returns an error with available files if a name is not found.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "metadata/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `password` | string | no | Password for protected documents |

## Example call

```json
{
  "name": "read_metadata",
  "arguments": {
    "file": {
      "filePath": "report.pdf"
    }
  }
}
```

## Result

A JSON object of properties grouped by the metadata packages present in the file.

Use this when the whole picture matters — a privacy review, an audit, a "what is in this file" question. When you only want one property, [`search_metadata`]({{< ref "metadata/mcp/tools-reference/search-metadata.md" >}}) returns less and reads better.

**In evaluation mode this result is truncated to the first five document properties** without saying so. Confirm the licence before drawing conclusions from an empty-looking file.

On failure the text starts with `Metadata read failed for`, followed by the exception type and message.

## Example prompts

* *"What metadata does report.pdf carry?"*
* *"Who created this document and when?"*
* *"Show me the EXIF data from this photo."*
* *"List the custom properties on this spreadsheet."*

See it used end-to-end: [Audit document metadata]({{< ref "metadata/mcp/use-cases/audit-document-metadata.md" >}}).
