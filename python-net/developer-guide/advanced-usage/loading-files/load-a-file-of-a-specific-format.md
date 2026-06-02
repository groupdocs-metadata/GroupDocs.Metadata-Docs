---
id: load-a-file-of-a-specific-format
url: metadata/python-net/load-a-file-of-a-specific-format
title: Load a file of a specific format
weight: 3
description: "This example demonstrates how to load a file of a particular format using GroupDocs.Metadata for Python via .NET."
keywords: load a file, specific format
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example demonstrates how to load a file of a particular format. Explicitly specifying the format through `LoadOptions` can spare the time spent on automatic format detection.

{{< tabs "load-a-file-of-a-specific-format">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.common import FileFormat
from groupdocs.metadata.options import LoadOptions


def loading_file_of_specific_format():
    load_options = LoadOptions(FileFormat.SPREADSHEET)

    with Metadata("input.xlsx", load_options) as metadata:
        root = metadata.get_root_package()
        # Use format-specific properties to extract or edit metadata
        print(f"Author: {root.document_properties.author}")


if __name__ == "__main__":
    loading_file_of_specific_format()
```
{{< /tab >}}
{{< tab "input.xlsx" >}}
{{< tab-text >}}
`input.xlsx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/loading-files/load-a-file-of-a-specific-format/input.xlsx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "loading-file-of-specific-format.txt" >}}  
```text
Author: gena
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/loading-files/load-a-file-of-a-specific-format/loading_file_of_specific_format/loading-file-of-specific-format.txt)
{{< /tab >}}
{{< /tabs >}}

## See also

- [Load a password-protected document]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/load-a-password-protected-document.md" >}})
- [Loading files]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
