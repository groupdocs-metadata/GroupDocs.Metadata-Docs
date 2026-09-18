---
id: mcp-tool-write-metadata
url: metadata/mcp/tools-reference/write-metadata
title: write_metadata
weight: 3
description: "The write_metadata MCP tool sets, changes, or adds a single metadata property on a document and saves the updated file."
keywords: write_metadata MCP tool, set document author AI, edit document properties agent, add title metadata MCP
productName: GroupDocs.Metadata MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

`write_metadata` sets, changes, or adds **one** metadata property and saves the updated file. Example prompt: *"Set the author of report.pdf to the Research team and give it a proper title"* — which is two calls, chained.

**Tool description (as the AI agent sees it):**

> Sets, changes, or adds a single metadata property on a document and saves the updated file to storage. Call this whenever the user wants to write a property value — 'set the author of report.pdf to ABC', 'add title "Q3 Report" to file.docx', 'change the subject', 'put my company name in the metadata'. `property` is one of Author, Title, Subject, Keywords, Comments, Copyright, Company, Manager; `value` is the text to write; `mode` is 'set' (replace, default) or 'add' (append, for list fields like Keywords). The tool maps the friendly name to the correct field per format automatically (PDF Info/XMP, Office document properties, EXIF/IPTC for images) and creates the field if it is absent. Supports PDF, DOCX, XLSX, PPTX, JPEG, PNG, TIFF and 100+ more formats. Returns a saved-path message plus how many fields changed; if the property does not apply to the file's format it reports 0 changed. On failure the response text starts with 'Metadata write failed for'.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "metadata/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `property` | string | yes | Property to write: Author, Title, Subject, Keywords, Comments, Copyright, Company, or Manager |
| `value` | string | yes | The text value to write |
| `mode` | string | no | 'set' to replace the value (default), or 'add' to append (for list fields like Keywords) |
| `password` | string | no | Password for protected documents |

## Example call

```json
{
  "name": "write_metadata",
  "arguments": {
    "file": {
      "filePath": "report.pdf"
    },
    "property": "Author",
    "value": "Research Team"
  }
}
```

## Result

A saved-path message naming the updated file.

One property per call. When several need changing, the agent must pass the **result** of each call into the next, or earlier edits are lost — each call starts from the file it was given.

On failure the text starts with `Metadata write failed for`, followed by the exception type and message. A property a format does not support fails here rather than silently doing nothing.

## Example prompts

* *"Set the author of report.pdf to the Research team."*
* *"Give this document the title 'Q3 Report'."*
* *"Add a custom property Reviewer with my name."*
