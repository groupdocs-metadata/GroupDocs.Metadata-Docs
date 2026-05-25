---
id: get-document-info
url: metadata/python-net/get-document-info
title: Get document info
weight: 1
description: GroupDocs.Metadata for Python via .NET lets you read basic file information — format, extension, MIME type, page count, size, and encryption state.
keywords: meta information, document info, file format, mime type, page count
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
GroupDocs.Metadata allows users to get document information which includes:

*   __File format__ (detected by the internal structure)
*   __File extension__
*   __MIME type__
*   __Number of pages__
*   __File size__
*   A __value__ indicating whether a file is encrypted

The following code sample demonstrates how to extract basic format information from a file.

{{< tabs "get-document-info">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def get_document_info():
    # Open the file (the context manager releases it on exit)
    with Metadata("input.xlsx") as metadata:
        # Read basic information detected from the file's internal structure
        info = metadata.get_document_info()
        # Format, extension and MIME type come from the file_type descriptor
        print(f"File format: {info.file_type.file_format}")
        print(f"File extension: {info.file_type.extension}")
        print(f"MIME Type: {info.file_type.mime_type}")
        # Page/size/encryption details are exposed directly on the info object
        print(f"Number of pages: {info.page_count}")
        print(f"Document size: {info.size} bytes")
        print(f"Is document encrypted: {info.is_encrypted}")


if __name__ == "__main__":
    get_document_info()
```
{{< /tab >}}
{{< tab "input.xlsx" >}}
{{< tab-text >}}
`input.xlsx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/basic-usage/get-document-info/input.xlsx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "get-document-info.txt" >}}  
```text
File format: 2
File extension: .xlsx
MIME Type: application/vnd.openxmlformats-officedocument.spreadsheetml.sheet
Number of pages: 2
Document size: 1677676 bytes
Is document encrypted: False
```
[Download full output](/metadata/python-net/_output_files/developer-guide/basic-usage/get-document-info/get_document_info/get-document-info.txt)
{{< /tab >}}
{{< /tabs >}}

## More resources

### Advanced usage topics

To learn more about library features and get familiar how to manage metadata and more, please refer to the [advanced usage section]({{< ref "metadata/python-net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
