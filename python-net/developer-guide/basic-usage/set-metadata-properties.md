---
id: set-metadata-properties
url: metadata/python-net/set-metadata-properties
title: Set metadata properties
weight: 4
description:  The set_properties method updates or adds metadata. Use it to add metadata to photos and PDFs or to update data in MP3 files with GroupDocs.Metadata for Python via .NET.
keywords: add metadata to pdf, add metadata to photos, add metadata to mp3, add metadata
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
## Update or add metadata properties satisfying a predicate

The **set_properties** method combines two operations: add and update. If an existing property satisfies the specified predicate, its value is updated. If a known property that satisfies the predicate is missing from a metadata package, it is added to the appropriate package.

The code snippet below demonstrates a basic usage scenario of the **set_properties** method.

1.  **Open** a file to update
2.  Specify a predicate that selects the metadata properties to add/update
3.  Specify the value you would like to write
4.  Check the actual number of added/updated properties
5.  **Save** the changes

{{< tabs "set-metadata-properties">}}
{{< tab "Python" >}}
```python
from datetime import datetime

from groupdocs.metadata import Metadata
from groupdocs.metadata.common import PropertyValue
from groupdocs.metadata.tagging import Tags


def set_metadata_properties():
    with Metadata("input.vsdx") as metadata:
        # The value to write into every matching property
        property_value = PropertyValue(datetime.now())
        # set_properties = add-or-update: the predicate selects the
        # "created" and "modified" date/time properties across all packages
        affected = metadata.set_properties(
            lambda p: Tags.time.created in list(p.tags)
            or Tags.time.modified in list(p.tags),
            property_value,
        )
        print(f"Properties set: {affected}")
        # Persist the changes to a new file
        metadata.save("output.vsdx")


if __name__ == "__main__":
    set_metadata_properties()
```
{{< /tab >}}
{{< tab "input.vsdx" >}}
{{< tab-text >}}
`input.vsdx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/basic-usage/set-metadata-properties/input.vsdx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.vsdx" >}}  
```text
Binary file (VSDX, 35 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/basic-usage/set-metadata-properties/set_metadata_properties/output.vsdx)
{{< /tab >}}
{{< /tabs >}}

As a result, we update all existing metadata properties that hold the date the document was created/updated. If a metadata package doesn't contain such properties but they belong in its structure, they are added.

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
