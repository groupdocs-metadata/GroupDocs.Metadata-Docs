---
id: mcp-tool-remove-metadata
url: metadata/mcp/tools-reference/remove-metadata
title: remove_metadata
weight: 4
description: "The remove_metadata MCP tool strips metadata from a document — everything by default, or only the categories you name — and saves a cleaned copy."
keywords: remove_metadata MCP tool, strip metadata before sharing, clean document properties AI, remove EXIF GPS agent
productName: GroupDocs.Metadata MCP Server
generated: true
serverVersion: 26.9.0
toc: True
---

`remove_metadata` strips metadata and saves a **cleaned copy**. With no `categories` it removes everything removable — the usual *"clean this before I send it"*. Pass `categories` to remove only certain kinds. Example prompt: *"Strip all metadata from these files before I share them"*.

**Tool description (as the AI agent sees it):**

> Removes metadata from a document and saves a cleaned copy to storage. By default (no categories) it strips ALL removable metadata — use this for 'remove all metadata before sharing'. To remove only SPECIFIC kinds, pass `categories` (one or more of): gps (location/geotags), author (creator/editor), comments, company, dates, software (the 'created with' tool fingerprint), copyright, keywords, personal (best-effort PII bundle: people + company + location). Call this whenever the user asks to remove, strip, clean, or redact metadata — e.g. 'remove GPS from photo.jpg' or 'strip the author from report.pdf'. Supports PDF, DOCX, XLSX, PPTX, JPEG, PNG, TIFF and 100+ more formats. Returns a saved-path message with the number of properties removed. On failure the response text starts with 'Metadata removal failed for'.

## Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `file` | object | yes |  — [FileInput shape]({{< ref "metadata/mcp/tools-reference/_index.md#the-fileinput-shape" >}}) |
| `categories` | array | no | Optional: remove only specific kinds — any of gps, author, comments, company, dates, software, copyright, keywords, personal. Omit (or 'all') to remove every removable property. |
| `password` | string | no | Password for protected documents |

## Example call

```json
{
  "name": "remove_metadata",
  "arguments": {
    "file": {
      "filePath": "report.pdf"
    }
  }
}
```

## Result

A saved-path message naming the cleaned file. The original is untouched — which is the point, and also the trap: **share the cleaned copy, not the original**.

What "everything" means is bounded by what the engine can remove: document properties, EXIF/XMP/IPTC packages, and similar. Information that is structurally part of the document — tracked changes, embedded objects, earlier revisions — is not metadata and is not removed here.

On failure the text starts with `Metadata removal failed for`, followed by the exception type and message.

## Example prompts

* *"Strip all metadata from this file before I share it."*
* *"Remove just the EXIF data from these photos."*
* *"Clean the author and company fields but keep the dates."*

See it used end-to-end: [Strip metadata before sharing]({{< ref "metadata/mcp/use-cases/strip-metadata-before-sharing.md" >}}).
