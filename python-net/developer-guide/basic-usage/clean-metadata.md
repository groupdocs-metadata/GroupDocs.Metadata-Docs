---
id: clean-metadata
url: metadata/python-net/clean-metadata
title: Clean metadata
weight: 5
description: Sometimes you just need to remove all metadata properties without applying any filters. The best way to do this is the sanitize method of GroupDocs.Metadata for Python via .NET.
keywords: clean metadata, metadata, sanitize
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
## Remove all recognized metadata properties from a file

Sometimes you just need to remove all metadata properties without applying any filters. The best way to do this is the **sanitize** method.

This example demonstrates how to remove all detected metadata packages/properties.

1.  **Load** a file to clean
2.  Call the **sanitize** method
3.  Check the actual number of removed packages/properties
4.  **Save** the changes

{{< tabs "clean-metadata">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def clean_metadata():
    # Open the file to clean
    with Metadata("input.pdf") as metadata:
        # sanitize() removes every detected metadata property in one call
        affected = metadata.sanitize()
        print(f"Properties removed: {affected}")
        # Write the cleaned document to a new file
        metadata.save("output.pdf")


if __name__ == "__main__":
    clean_metadata()
```
{{< /tab >}}
{{< tab "input.pdf" >}}
{{< tab-text >}}
`input.pdf` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/basic-usage/clean-metadata/input.pdf) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.pdf" >}}  
```text
Binary file (PDF, 382 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/basic-usage/clean-metadata/clean_metadata/output.pdf)
{{< /tab >}}
{{< /tabs >}}

As a result, we get a sanitized version of the original file.

## More resources

### Advanced usage topics

To learn more about library features and get familiar how to manage metadata and more, please refer to the [advanced usage section]({{< ref "metadata/python-net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
