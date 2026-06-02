---
id: loading-files
url: metadata/python-net/loading-files
title: Loading files
weight: 1
description: "Load files from a local disk, a stream, a specific format, or a password-protected document with GroupDocs.Metadata for Python via .NET."
keywords: load file, load from disk, load from stream, specific format, password-protected, LoadOptions, python
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
The `Metadata` constructor accepts a file path, a binary stream, or a URI, and auto-detects the format. Pass an optional `LoadOptions` to override the format or supply a password.

| `LoadOptions` property | Type | Purpose |
|---|---|---|
| `file_format` | `FileFormat` | Skip auto-detection by stating the format explicitly. |
| `password` | `str` | Open a password-protected document. |

For more details, please refer to the following guides:

- [Load from a local disk]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/load-from-a-local-disk.md" >}}) — open a file by its path.
- [Load from a stream]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/load-from-a-stream.md" >}}) — open a readable binary stream (`open(..., "rb")` or `io.BytesIO`).
- [Load a file of a specific format]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/load-a-file-of-a-specific-format.md" >}}) — use `LoadOptions(FileFormat.…)` to skip format detection.
- [Load a password-protected document]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/load-a-password-protected-document.md" >}}) — provide the password via `LoadOptions`.
