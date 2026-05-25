---
id: quick-start-guide
url: metadata/python-net/getting-started/quick-start-guide
title: Quick Start Guide
linkTitle: Quick Start Guide
second_title: A simple example of how to use GroupDocs.Metadata for Python via .NET
weight: 2
keywords: quick start, hello world, get started, read metadata, remove metadata, document info, pip install, venv, virtual environment, GroupDocs.Metadata, python
description: "Set up a virtual environment, install groupdocs-metadata-net, and run three minimal examples — read metadata, remove all metadata, and inspect a document — in under five minutes."
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
toc: True
---

This guide gives a quick overview of how to set up and start using GroupDocs.Metadata for Python via .NET. The library lets you read, edit, and remove metadata from documents, images, audio, and video with minimal configuration.

## Prerequisites

To proceed, make sure you have:

1. A configured environment as described in the [System Requirements]({{< ref "/metadata/python-net/getting-started/system-requirements.md" >}}) topic.
2. Optionally, a [Temporary License](https://purchase.groupdocs.com/temporary-license/) to test all product features (saving files requires a license).

## Set Up Your Development Environment

For best practices, use a virtual environment to manage dependencies.

### Create and Activate a Virtual Environment

Create a virtual environment:

{{< tabs "create-venv">}}
{{< tab "Windows" >}}
```ps
py -m venv .venv
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m venv .venv
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m venv .venv
```
{{< /tab >}}
{{< /tabs >}}

Activate it:

{{< tabs "activate-venv">}}
{{< tab "Windows" >}}
```ps
.venv\Scripts\activate
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
source .venv/bin/activate
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
source .venv/bin/activate
```
{{< /tab >}}
{{< /tabs >}}

### Install the `groupdocs-metadata-net` package

{{< tabs "install-package">}}
{{< tab "Windows" >}}
```ps
py -m pip install groupdocs-metadata-net
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m pip install groupdocs-metadata-net
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m pip install groupdocs-metadata-net
```
{{< /tab >}}
{{< /tabs >}}

## Example 1: Read metadata

Open a file and print all detected metadata properties.

{{< tabs "qs-read-metadata">}}
{{< tab "Python" >}}
```python
import os

from groupdocs.metadata import License, Metadata


def read_metadata():
    # Apply a license if one is present next to the script
    license_path = os.path.abspath("./GroupDocs.Metadata.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Metadata("./input.docx") as metadata:
        # `lambda p: True` matches every property in the file
        for prop in metadata.find_properties(lambda p: True):
            print(f"{prop.name} = {prop.value}")


if __name__ == "__main__":
    read_metadata()
```
{{< /tab >}}
{{< tab "input.docx" >}}
{{< tab-text >}}
`input.docx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/getting-started/quick-start-guide/input.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "read-metadata.txt" >}}  
```text
FileType = GroupDocs.Metadata.Formats.Document.WordProcessingTypePackage
DocumentProperties = GroupDocs.Metadata.Formats.Document.WordProcessingPackage
DublinCore = GroupDocs.Metadata.Standards.DublinCore.DublinCorePackage
InspectionPackage = GroupDocs.Metadata.Formats.Document.WordProcessingInspectionPackage
DocumentStatistics = GroupDocs.Metadata.Formats.Document.DocumentStatistics
FileFormat = 3
MimeType = application/vnd.openxmlformats-officedocument.wordprocessingml.document
Extension = .do
[TRUNCATED]
```
[Download full output](/metadata/python-net/_output_files/getting-started/quick-start-guide/read_metadata/read-metadata.txt)
{{< /tab >}}
{{< /tabs >}}

## Example 2: Remove all metadata

Strip every detected property and save a clean copy. Saving requires a license.

{{< tabs "qs-remove-metadata">}}
{{< tab "Python" >}}
```python
import os

from groupdocs.metadata import License, Metadata


def remove_metadata():
    license_path = os.path.abspath("./GroupDocs.Metadata.lic")
    if os.path.exists(license_path):
        License().set_license(license_path)

    with Metadata("./input.docx") as metadata:
        # sanitize() strips every detected property; save() needs a license
        removed = metadata.sanitize()
        print(f"Removed {removed} properties")
        metadata.save("./clean.docx")


if __name__ == "__main__":
    remove_metadata()
```
{{< /tab >}}
{{< tab "input.docx" >}}
{{< tab-text >}}
`input.docx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/getting-started/quick-start-guide/input.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "clean.docx" >}}  
```text
Binary file (DOCX, 14 KB)
```
[Download full output](/metadata/python-net/_output_files/getting-started/quick-start-guide/remove_metadata/clean.docx)
{{< /tab >}}
{{< /tabs >}}

## Example 3: Inspect a document

Read basic file information without parsing the whole metadata tree.

{{< tabs "qs-document-info">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def document_info():
    with Metadata("./input.docx") as metadata:
        # get_document_info() reads basic facts without walking the metadata tree
        info = metadata.get_document_info()
        print(f"Format: {info.file_type.file_format}")
        print(f"MIME type: {info.file_type.mime_type}")
        print(f"Pages: {info.page_count}")
        print(f"Size: {info.size} bytes")
        print(f"Encrypted: {info.is_encrypted}")


if __name__ == "__main__":
    document_info()
```
{{< /tab >}}
{{< tab "input.docx" >}}
{{< tab-text >}}
`input.docx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/getting-started/quick-start-guide/input.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "document-info.txt" >}}  
```text
Format: 3
MIME type: application/vnd.openxmlformats-officedocument.wordprocessingml.document
Pages: 1
Size: 17952 bytes
Encrypted: False
```
[Download full output](/metadata/python-net/_output_files/getting-started/quick-start-guide/document_info/document-info.txt)
{{< /tab >}}
{{< /tabs >}}

## Next Steps

After completing the basics, explore additional resources:
- [Supported File Formats]({{< ref "/metadata/python-net/getting-started/supported-document-formats.md" >}}): review the full list of supported file types.
- [Developer Guide]({{< ref "/metadata/python-net/developer-guide/_index.md" >}}): runnable examples for every API surface.
- [Licensing]({{< ref "/metadata/python-net/getting-started/evaluation-limitations-and-licensing.md" >}}): details on licensing and evaluation.
- [Technical Support]({{< ref "metadata/python-net/technical-support" >}}): contact support if you run into issues.
