---
id: save-a-modified-file-to-a-specified-location
url: metadata/python-net/save-a-modified-file-to-a-specified-location
title: Save a modified file to a specified location
weight: 2
description: "This article shows how to save a document to a specified location on a local disk using GroupDocs.Metadata for Python via .NET."
keywords: save a document to a specified location
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example shows how to save a modified file to a specified location on a local disk by passing an output path to the `save` method.

{{< tabs "save-a-modified-file-to-a-specified-location">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def save_file_to_specified_location():
    with Metadata("input.jpg") as metadata:
        # Edit or remove metadata here
        removed = metadata.sanitize()
        print(f"Removed {removed} properties")

        metadata.save("output.jpg")


if __name__ == "__main__":
    save_file_to_specified_location()
```
{{< /tab >}}
{{< tab "input.jpg" >}}
{{< tab-text >}}
`input.jpg` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/saving-files/save-a-modified-file-to-a-specified-location/input.jpg) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.jpg" >}}  
```text
Binary file (JPG, 795 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/saving-files/save-a-modified-file-to-a-specified-location/save_file_to_specified_location/output.jpg)
{{< /tab >}}
{{< /tabs >}}

## See also

- [Save a modified file to a stream]({{< ref "metadata/python-net/developer-guide/advanced-usage/saving-files/save-a-modified-file-to-a-stream.md" >}})
- [Saving files]({{< ref "metadata/python-net/developer-guide/advanced-usage/saving-files/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
