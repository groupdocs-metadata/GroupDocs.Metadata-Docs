---
id: remove-metadata-properties
url: metadata/python-net/remove-metadata-properties
title: Remove metadata properties
weight: 3
description: The easiest way to remove metadata properties from a file is to use the corresponding tags that let you locate the desired properties across all metadata packages with GroupDocs.Metadata for Python via .NET.
keywords: remove metadata, metadata
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
## Remove metadata properties using various criteria

The easiest way to remove metadata properties from a file is to use the corresponding tags that let you locate the desired properties across all metadata packages. But sometimes it's necessary to remove metadata entries that have a particular value. With a Python predicate you can find and remove properties that satisfy a condition as complex as you need.

The following example demonstrates how to remove specific metadata properties using a combination of criteria.

1.  **Load** a file to update
2.  Pass a predicate to the **remove_properties** method to find and remove the desired properties
3.  Check the number of properties that were actually removed (the return value of **remove_properties**)
4.  **Save** the changes

{{< tabs "remove-metadata-properties">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.common import MetadataPropertyType
from groupdocs.metadata.tagging import Tags


def remove_metadata_properties():
    # Remove all the properties satisfying the predicate:
    #   the property carries the "author" tag, OR
    #   the property carries the "last editor" tag, OR
    #   the property is a string whose value equals "John"
    #   (to wipe any mention of John from the detected metadata)
    with Metadata("input.docx") as metadata:
        affected = metadata.remove_properties(
            lambda p: Tags.person.creator in list(p.tags)
            or Tags.person.editor in list(p.tags)
            or (p.value.type == MetadataPropertyType.STRING and str(p.value) == "John")
        )
        print(f"Properties removed: {affected}")
        metadata.save("output.docx")


if __name__ == "__main__":
    remove_metadata_properties()
```
{{< /tab >}}
{{< tab "input.docx" >}}
{{< tab-text >}}
`input.docx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/basic-usage/remove-metadata-properties/input.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.docx" >}}  
```text
Binary file (DOCX, 14 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/basic-usage/remove-metadata-properties/remove_metadata_properties/output.docx)
{{< /tab >}}
{{< /tabs >}}

As a result of running the code snippet above, we remove all mentions of the document author/editor and all other string metadata properties whose value is "John".

For more information on searching metadata, please refer to the following articles:

*   [Extracting metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/extracting-metadata.md" >}})
*   [Removing metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/removing-metadata.md" >}})
*   [Adding metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/adding-metadata.md" >}})

## More resources

### Advanced usage topics

To learn more about library features and get familiar how to manage metadata and more, please refer to the [advanced usage section]({{< ref "metadata/python-net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
