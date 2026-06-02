---
id: load-from-a-local-disk
url: metadata/python-net/load-from-a-local-disk
title: Load from a local disk
weight: 1
description: "Learn how to open a file from local disk using GroupDocs.Metadata for Python via .NET."
keywords: load file from local disk
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
The following example demonstrates how to load a file from a local disk by passing its path to the `Metadata` constructor.

{{< tabs "load-from-a-local-disk">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def load_from_local_disk():
    # Absolute or relative path to your document
    with Metadata("input.docx") as metadata:
        # Extract, edit or remove metadata here
        info = metadata.get_document_info()
        print(f"Loaded {info.file_type.file_format} ({info.size} bytes)")


if __name__ == "__main__":
    load_from_local_disk()
```
{{< /tab >}}
{{< tab "input.docx" >}}
{{< tab-text >}}
`input.docx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/loading-files/load-from-a-local-disk/input.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "load-from-local-disk.txt" >}}  
```text
Loaded 3 (17952 bytes)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/loading-files/load-from-a-local-disk/load_from_local_disk/load-from-local-disk.txt)
{{< /tab >}}
{{< /tabs >}}

## See also

- [Load from a stream]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/load-from-a-stream.md" >}})
- [Loading files]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
