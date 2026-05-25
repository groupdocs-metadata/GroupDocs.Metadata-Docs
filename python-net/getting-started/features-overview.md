---
id: features-overview
url: metadata/python-net/features-overview
title: Features Overview
linkTitle: Features Overview
weight: 1
description: "Key features of GroupDocs.Metadata for Python via .NET — read, edit, remove, and export metadata; EXIF, IPTC, and XMP standards; ID3 audio tags; document inspection; and AI-pipeline integration."
keywords: features, metadata, read metadata, edit metadata, remove metadata, export metadata, EXIF, IPTC, XMP, ID3, document properties, python
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
toc: True
---

## Overview

GroupDocs.Metadata for Python via .NET reads, edits, removes, and exports metadata across **documents, images, audio, video, and many other formats** — Microsoft Office, PDF, OpenDocument, images (JPEG, PNG, PSD, TIFF), audio (MP3, WAV), video, fonts, e-books, archives, and more. It exposes the major metadata standards (XMP, EXIF, IPTC, Image Resource Blocks, ID3) and format-specific properties through one unified, predicate-driven API. It runs entirely on-premise, requires no Microsoft Office installation, and ships as a pre-built wheel on Windows, Linux, and macOS.

See the full list of [supported formats]({{< ref "/metadata/python-net/getting-started/supported-document-formats.md" >}}) or browse the [Developer Guide]({{< ref "/metadata/python-net/developer-guide/_index.md" >}}) for runnable examples of every API surface.

## Reading and Searching Metadata

Read every property in a file, or search for specific properties using a plain Python predicate (`lambda p: ...`) — no specification objects required. You can match on a property's name, value, type, or [tags]({{< ref "/metadata/python-net/developer-guide/advanced-usage/working-with-interpreted-values.md" >}}).

- [Find metadata properties]({{< ref "/metadata/python-net/developer-guide/basic-usage/find-metadata-properties.md" >}}) — locate properties that match a predicate, across any format.
- [Extract metadata]({{< ref "/metadata/python-net/developer-guide/advanced-usage/extracting-metadata.md" >}}) — pull properties from the native, document, and standards packages.
- [Get known property descriptors]({{< ref "/metadata/python-net/developer-guide/advanced-usage/getting-known-property-descriptors.md" >}}) — discover the strongly-typed properties a package exposes.
- [Work with interpreted values]({{< ref "/metadata/python-net/developer-guide/advanced-usage/working-with-interpreted-values.md" >}}) — read human-friendly, interpreted forms of raw property values.

## Editing Metadata

Add new properties or update existing ones in a unified way. Predefined **tags** let you set common properties (author, creation date, title) consistently regardless of the underlying format.

- [Set metadata properties]({{< ref "/metadata/python-net/developer-guide/basic-usage/set-metadata-properties.md" >}}) — add or update every property matching a predicate.
- [Add metadata]({{< ref "/metadata/python-net/developer-guide/advanced-usage/adding-metadata.md" >}}) — add missing properties (for example, a last-printed date) when the format supports them.

## Removing and Cleaning Metadata

Remove particular properties that match a predicate, or strip every detected property in a single call — ideal for privacy and compliance workflows.

- [Remove metadata properties]({{< ref "/metadata/python-net/developer-guide/basic-usage/remove-metadata-properties.md" >}}) — delete only the properties that match a predicate.
- [Clean metadata]({{< ref "/metadata/python-net/developer-guide/basic-usage/clean-metadata.md" >}}) — `sanitize()` removes every detected property at once.
- [Remove metadata (advanced)]({{< ref "/metadata/python-net/developer-guide/advanced-usage/removing-metadata.md" >}}) — remove whole metadata packages from a file.

## Working with Metadata Standards

Read and write the metadata standards used by images and many other formats:

- [EXIF]({{< ref "/metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata.md" >}}) — read, update, and remove EXIF tags in JPEG, TIFF, PNG, WEBP, PSD, and more.
- [IPTC IIM]({{< ref "/metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata.md" >}}) — read and edit IPTC datasets in JPEG, TIFF, and PSD.
- [XMP]({{< ref "/metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata.md" >}}) — read, update, and add custom XMP packages across formats.

Image Resource Blocks (IRB) and format-specific native packages are exposed through the same root metadata tree.

## Audio Metadata

Manage audio tags through the root package: **ID3v1**, **ID3v2**, **Lyrics3**, and **APE**, plus MPEG audio technical properties. Read or edit titles, artists, albums, and other tag fields, then [export]({{< ref "/metadata/python-net/developer-guide/advanced-usage/working-with-metadata-properties/exporting-metadata-properties.md" >}}) or strip them as needed.

## Document Inspection and File Type Detection

Read basic facts about a file without walking its full metadata tree — format, MIME type, page count, size, and encryption state — and detect a file's format by its internal structure rather than its extension.

- [Get document info]({{< ref "/metadata/python-net/developer-guide/basic-usage/get-document-info.md" >}}) — format, MIME type, page count, size, and encryption flag.

Office documents also expose inspection data such as user comments, form fields, hidden pages, revisions, digital signatures, and common statistics (word count, character count).

## Loading Files from Different Sources

The `Metadata` constructor accepts a file path, a binary file-like object, or a URI, so you can load files from local disk, in-memory buffers, or cloud storage.

- [Load from a local disk]({{< ref "/metadata/python-net/developer-guide/advanced-usage/loading-files/load-from-a-local-disk.md" >}})
- [Load from a stream]({{< ref "/metadata/python-net/developer-guide/advanced-usage/loading-files/load-from-a-stream.md" >}}) — `open("file.jpg", "rb")`, `io.BytesIO(data)`, or bytes fetched from S3, Azure Blob, or `requests`.
- [Load a file of a specific format]({{< ref "/metadata/python-net/developer-guide/advanced-usage/loading-files/load-a-file-of-a-specific-format.md" >}})
- [Load a password-protected document]({{< ref "/metadata/python-net/developer-guide/advanced-usage/loading-files/load-a-password-protected-document.md" >}})

## Saving Files

After editing, save back to the original source or to a new destination — a path or a stream.

- [Save to a specified location]({{< ref "/metadata/python-net/developer-guide/advanced-usage/saving-files/save-a-modified-file-to-a-specified-location.md" >}})
- [Save to a stream]({{< ref "/metadata/python-net/developer-guide/advanced-usage/saving-files/save-a-modified-file-to-a-stream.md" >}})
- [Save to the original source]({{< ref "/metadata/python-net/developer-guide/advanced-usage/saving-files/save-a-modified-file-to-the-original-source.md" >}})

{{< alert style="info" >}}
Saving a modified file requires a license. Without a license, the library runs in evaluation mode — it reads only the first few properties of each package and `save()` raises an "Evaluation only" exception. See [Evaluation Limitations and Licensing]({{< ref "/metadata/python-net/getting-started/evaluation-limitations-and-licensing.md" >}}).
{{< /alert >}}

## Exporting Metadata

Export the metadata tree — or a filtered subset of properties — to a spreadsheet or a structured data file for indexing, reporting, or downstream processing.

- [Export metadata properties]({{< ref "/metadata/python-net/developer-guide/advanced-usage/working-with-metadata-properties/exporting-metadata-properties.md" >}}) — write to **Excel, CSV, JSON, or XML** via `ExportManager`.

## AI and LLM Integration

GroupDocs.Metadata is a useful building block for AI document pipelines: extract metadata as structured data to enrich search indexes and LLM context. The `groupdocs-metadata-net` pip package ships an `AGENTS.md` file inside the wheel so AI coding assistants can discover the API surface automatically, and GroupDocs runs a public [MCP server](https://docs.groupdocs.com/mcp) for on-demand documentation lookups. See [Agents and LLM Integration]({{< ref "/metadata/python-net/agents-and-llm-integration.md" >}}) for the full story.

## On-Premise Deployment

No cloud calls, no outbound network traffic, no third-party software dependencies beyond what the OS already provides. The wheel is self-contained on Windows and ships its own native runtime libraries on Linux and macOS. See [System Requirements]({{< ref "/metadata/python-net/getting-started/system-requirements.md" >}}) for the short list of optional native packages (`libgdiplus`, fontconfig).
