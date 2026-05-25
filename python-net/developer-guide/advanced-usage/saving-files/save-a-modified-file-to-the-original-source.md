---
id: save-a-modified-file-to-the-original-source
url: metadata/python-net/save-a-modified-file-to-the-original-source
title: Save a modified file to the original source
weight: 1
description: "This article shows how to save the modified content back to the underlying source using GroupDocs.Metadata for Python via .NET."
keywords: how to save the modified content to the underlying source
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example shows how to save the modified content back to the underlying source. Call `save()` with no arguments — the changes are written back to the file (or stream) the document was loaded from.

{{< tabs "save-a-modified-file-to-the-original-source">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def save_file_to_original_source():
    with Metadata("input.ppt") as metadata:
        # Edit or remove metadata here
        removed = metadata.sanitize()
        print(f"Removed {removed} properties")

        # Saves the document back to the underlying source (stream or file)
        metadata.save()


if __name__ == "__main__":
    save_file_to_original_source()
```
{{< /tab >}}
{{< tab "input.ppt" >}}  
```text
Binary file (PPT, 172 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/saving-files/save-a-modified-file-to-the-original-source/save_file_to_original_source/input.ppt)
{{< /tab >}}
{{< /tabs >}}

## See also

- [Save a modified file to a specified location]({{< ref "metadata/python-net/developer-guide/advanced-usage/saving-files/save-a-modified-file-to-a-specified-location.md" >}})
- [Saving files]({{< ref "metadata/python-net/developer-guide/advanced-usage/saving-files/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
