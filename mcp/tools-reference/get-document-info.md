---
id: mcp-tool-get-document-info
url: metadata/mcp/tools-reference/get-document-info
title: get_document_info
weight: 5
description: "The get_document_info MCP tool returns file type, size, page count, and encryption status — a lightweight structural check that does not enumerate metadata."
keywords: get_document_info MCP, check if document encrypted, file type page count MCP, lightweight document check
productName: GroupDocs.Metadata MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

`get_document_info` returns the file type, size, page count, and **encryption status** without enumerating properties. It is the cheap triage call: what is this file, and can it even be opened? Example prompt: *"Is this PDF encrypted?"*

**Tool description (as the AI agent sees it):**

> Returns the file type, size, page count, and encryption status of a document as JSON — a lightweight structural check that does NOT enumerate metadata properties (use ReadMetadata for that). Supports PDF, DOCX, XLSX, PPTX, JPEG, PNG, TIFF, MP3, MP4, WAV, AVI, and 50+ more document, image, and media formats. Call this tool whenever the user asks about a document's format, page count, byte size, or whether it is encrypted, without needing the full metadata dump. Do NOT pre-check whether the file exists — just pass the filename the user provided. Returns a JSON object with fields `fileName`, `fileFormat` (engine-reported format name), `mimeType`, `pageCount`, `sizeBytes`, and `isEncrypted`. On failure, the response text starts with 'Document-info lookup failed for' followed by the underlying exception type, message, and inner-exception chain.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "metadata/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `password` | string | no | Password for protected documents |

## Example call

```json
{
  "name": "get_document_info",
  "arguments": {
    "file": {
      "filePath": "report.pdf"
    }
  }
}
```

## Result

A JSON object with the file type, size, page count, and encryption status.

The encryption flag is the useful one here: it explains why a metadata read might fail, and it lets an agent ask you for the password instead of guessing.

On failure the text starts with `Document-info lookup failed for`, followed by the exception type and message.

## Example prompts

* *"Is this PDF encrypted?"*
* *"What type and size is this file?"*
* *"How many pages, before I process the batch?"*
