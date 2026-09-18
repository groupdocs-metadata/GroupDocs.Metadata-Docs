---
id: mcp-supported-formats
url: metadata/mcp/supported-formats
title: Supported formats
weight: 4
description: "The MCP server exposes the full GroupDocs.Metadata engine: documents, images, audio, video, archives and more — every format the .NET library can read and write metadata for."
keywords: MCP server supported formats, pdf metadata MCP, EXIF image metadata MCP, audio video metadata agent
productName: GroupDocs.Metadata MCP Server
toc: True
---

The MCP server exposes the **full GroupDocs.Metadata engine**: every format the .NET library can read and write metadata for is available to your AI agent. The canonical matrix lives in the library documentation: [supported document formats]({{< ref "metadata/net/getting-started/supported-document-formats.md" >}}).

The families that matter in practice:

* **Documents** — PDF, DOCX/DOC, XLSX/XLS, PPTX/PPT, Visio, OpenDocument: author, title, company, dates, and custom properties.
* **Images** — JPEG, PNG, TIFF, WebP, PSD and more: **EXIF** (including camera and GPS), **XMP**, and **IPTC**.
* **Audio and video** — MP3, WAV, MP4, AVI, MOV: ID3 tags and container metadata.
* **Archives and others** — ZIP and further formats the engine supports.

**Not every property exists in every format.** Writing a property a format has no place for fails with a clear message rather than silently doing nothing — which is the behaviour you want when an agent is editing files on your behalf.

Two format-specific evaluation limits worth knowing: **XMP** exposes only the first two schemes, and **EXIF GPS data and thumbnails are unavailable**, until a license is applied. A photo can therefore look free of location data when it is not — see [Licensing]({{< ref "metadata/mcp/getting-started/licensing.md" >}}).
