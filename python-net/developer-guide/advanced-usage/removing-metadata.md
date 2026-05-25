---
id: removing-metadata
url: metadata/python-net/removing-metadata
title: Removing metadata
weight: 3
description: "Locate and remove the metadata properties you don't want — by tag, category, name, type or value — with GroupDocs.Metadata for Python via .NET."
keywords: removing metadata, remove metadata properties, GroupDocs.Metadata, python
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
Not all metadata properties extracted from a file are marked with tags. Some file formats and metadata standards allow fully custom properties that can't be tagged by the library, because their purpose is not clearly defined in the corresponding specification. In such cases you can use the property name (or any other attribute) to locate and remove it. The following example demonstrates an advanced removal scenario.

1.  **Load** a file to modify
2.  Pass a search predicate to the **remove_properties** method
3.  Check the number of properties that were actually removed
4.  **Save** the changes

{{< tabs "removing-metadata">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.tagging import Tags


def removing_metadata():
    with Metadata("input.docx") as metadata:
        # Remove every property whose tags fall into the "content" category
        affected = metadata.remove_properties(
            lambda p: any(tag.category == Tags.content for tag in p.tags)
        )
        print(f"Affected properties: {affected}")
        metadata.save("output.docx")


if __name__ == "__main__":
    removing_metadata()
```
{{< /tab >}}
{{< tab "input.docx" >}}
{{< tab-text >}}
`input.docx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/removing-metadata/input.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.docx" >}}  
```text
Binary file (DOCX, 13 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/removing-metadata/removing_metadata/output.docx)
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
