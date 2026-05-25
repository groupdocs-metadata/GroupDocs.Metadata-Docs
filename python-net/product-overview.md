---
id: product-overview
url: metadata/python-net/product-overview
title: GroupDocs.Metadata for Python via .NET Overview
linkTitle: Product overview
weight: 1
description: "GroupDocs.Metadata for Python via .NET reads, edits, removes, and exports metadata across documents, images, audio, and video — EXIF, IPTC, XMP, ID3, and document properties — through one unified, predicate-driven API."
keywords: metadata, read, edit, update, remove, export, EXIF, IPTC, XMP, ID3, document properties, PDF, DOCX, XLSX, PPTX, image, audio, python
productName: GroupDocs.Metadata for Python via .NET
toc: True
---

## What is GroupDocs.Metadata?

GroupDocs.Metadata for Python via .NET is a native Python library that reads, edits, removes, and exports metadata across **documents, images, audio, video, and many other formats**. It works with the most notable metadata standards — XMP, EXIF, IPTC, Image Resource Blocks, and ID3 — as well as format-specific and built-in document properties, and exposes them all through one unified, predicate-driven API regardless of file format. It runs entirely on-premise, requires no Microsoft Office installation, and ships as a pre-built wheel on Windows, Linux, and macOS.

Typical uses include:

- **Privacy and compliance** — strip author, GPS location, comments, and hidden data from files before they leave your organization.
- **Digital asset management** — read and edit EXIF, IPTC, and XMP fields on images to drive search, tagging, and cataloguing.
- **AI / content preprocessing** — extract metadata as structured data (JSON, CSV) to enrich search indexes and LLM context. See [Agents and LLM Integration]({{< ref "/metadata/python-net/agents-and-llm-integration.md" >}}).
- **Auditing** — inspect documents for format, MIME type, encryption state, and statistics without modifying them.
- **Batch normalization** — set or update properties (titles, dates, copyright) across many files with a single predicate.

With its powerful and straightforward API, you can:

*   Work with the most popular metadata standards: XMP, EXIF, IPTC, Image Resource Blocks, ID3, document properties, etc.
*   Manage audio metadata: ID3 tags (ID3v1, ID3v2), Lyrics3 tag, APE.
*   Create, modify, and remove metadata with a few lines of code.
*   Identify built-in, custom, and hidden metadata.
*   Work with password-protected documents.
*   Detect the format and MIME type of a loaded file by its internal structure.
*   Use predefined tags to manipulate metadata properties in a unified way across all supported formats.
*   Detect and remove digital signatures associated with a loaded file.
*   Inspect office documents to extract user comments, form fields, hidden pages, etc.
*   Manage metadata in e-books, torrent files, archives, electronic business cards, saved emails, and more.

GroupDocs.Metadata runs across multiple platforms and operating systems: **Windows, Linux, and macOS** (Intel and Apple Silicon).

## Key Capabilities

| Capability | Description |
|---|---|
| **Read and search metadata** | [Find properties with a plain Python predicate]({{< ref "/metadata/python-net/developer-guide/basic-usage/find-metadata-properties.md" >}}) across any format. |
| **Edit and add metadata** | [Set, add, and update properties]({{< ref "/metadata/python-net/developer-guide/basic-usage/set-metadata-properties.md" >}}) matched by a predicate. |
| **Remove and sanitize** | [Remove selected properties]({{< ref "/metadata/python-net/developer-guide/basic-usage/remove-metadata-properties.md" >}}) or [strip everything in one call]({{< ref "/metadata/python-net/developer-guide/basic-usage/clean-metadata.md" >}}). |
| **Metadata standards** | Read and write [EXIF]({{< ref "/metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata.md" >}}), [IPTC IIM]({{< ref "/metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata.md" >}}), and [XMP]({{< ref "/metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata.md" >}}) on images. |
| **Audio tags** | ID3v1, ID3v2, Lyrics3, and APE tags in audio files. |
| **Document inspection** | [Read format, MIME type, page count, size, and encryption]({{< ref "/metadata/python-net/developer-guide/basic-usage/get-document-info.md" >}}) without editing. |
| **Export** | [Export the metadata tree to Excel, CSV, JSON, or XML]({{< ref "/metadata/python-net/developer-guide/advanced-usage/working-with-metadata-properties/exporting-metadata-properties.md" >}}). |
| **Load from anywhere** | [Local disk]({{< ref "/metadata/python-net/developer-guide/advanced-usage/loading-files/load-from-a-local-disk.md" >}}), [a stream]({{< ref "/metadata/python-net/developer-guide/advanced-usage/loading-files/load-from-a-stream.md" >}}), or [a specific format]({{< ref "/metadata/python-net/developer-guide/advanced-usage/loading-files/load-a-file-of-a-specific-format.md" >}}); [password-protected files]({{< ref "/metadata/python-net/developer-guide/advanced-usage/loading-files/load-a-password-protected-document.md" >}}) supported. |
| **Unified tags** | Predefined tags manipulate common properties (author, creation date, title) uniformly across every format. |
| **On-premise** | No cloud calls, no Microsoft Office install, no network traffic. |

## Quick Example

{{< tabs "quick-example">}}
{{< tab "Read metadata" >}}
```python
from groupdocs.metadata import Metadata


def quick_example():
    """Read every metadata property from a document."""
    with Metadata("./input.docx") as metadata:
        # `lambda p: True` matches every property in the file
        for prop in metadata.find_properties(lambda p: True):
            print(f"{prop.name} = {prop.value}")


if __name__ == "__main__":
    quick_example()
```
{{< /tab >}}
{{< tab "Remove all metadata" >}}
```python
from groupdocs.metadata import License, Metadata


def remove_all_metadata():
    """Strip every property and save a clean copy (saving requires a license)."""
    License().set_license("./GroupDocs.Metadata.lic")

    with Metadata("./input.pdf") as metadata:
        removed = metadata.sanitize()
        print(f"Removed {removed} properties")
        metadata.save("./clean.pdf")


if __name__ == "__main__":
    remove_all_metadata()
```
{{< /tab >}}
{{< /tabs >}}

## Where to Next

1. **Install the package** — [Installation]({{< ref "/metadata/python-net/getting-started/installation.md" >}}) walks through PyPI and offline wheel installation for Windows, Linux, and macOS.
2. **Run your first example** — the [Quick Start Guide]({{< ref "/metadata/python-net/getting-started/quick-start-guide.md" >}}) reads and removes metadata in a few minutes.
3. **Explore the examples** — [How to Run Examples]({{< ref "/metadata/python-net/getting-started/how-to-run-examples.md" >}}) clones the runnable repository and runs every documented scenario locally or in Docker.
4. **Use it in depth** — the [Developer Guide]({{< ref "/metadata/python-net/developer-guide/_index.md" >}}) covers reading, editing, removing, standards, loading, saving, and exporting.
5. **Plug it into AI pipelines** — [Agents and LLM Integration]({{< ref "/metadata/python-net/agents-and-llm-integration.md" >}}) explains the MCP server and the `AGENTS.md` shipped inside the wheel.

## Technical Support

If you encounter an issue while using GroupDocs.Metadata or have a technical question, feel free to create a post in our [Free Support Forum](https://forum.groupdocs.com/c/metadata/9). If free support is not sufficient, you can submit a ticket to our [Paid Support Helpdesk](https://helpdesk.groupdocs.com/).
