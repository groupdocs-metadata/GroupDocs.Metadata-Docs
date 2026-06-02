---
id: adding-metadata
url: metadata/python-net/adding-metadata
title: Adding metadata
weight: 5
description: "Add metadata properties — one of the most powerful features of the GroupDocs.Metadata for Python via .NET search engine."
keywords: adding metadata, adding metadata properties
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
Adding metadata properties is one of the most powerful features of the GroupDocs.Metadata search engine. When you call the **add_properties** method, it examines all available metadata packages and tries to pick up a known property that satisfies the specified predicate. Note that a property is added only to packages that fit the following criteria:

1.  Only existing metadata packages are affected. No new packages are created during this operation.
2.  There must be a known metadata property that fits the search condition but is actually missing from the package. The properties supported by a package are usually defined in the specification of the corresponding metadata standard.

{{< tabs "adding-metadata">}}
{{< tab "Python" >}}
```python
from datetime import datetime

from groupdocs.metadata import Metadata
from groupdocs.metadata.common import PropertyValue
from groupdocs.metadata.tagging import Tags


def adding_metadata():
    with Metadata("input.docx") as metadata:
        # Add the "last printed" date wherever it is a known but missing property
        property_value = PropertyValue(datetime.now())
        affected = metadata.add_properties(
            lambda p: Tags.time.printed in list(p.tags), property_value
        )
        print(f"Affected properties: {affected}")
        metadata.save("output.docx")


if __name__ == "__main__":
    adding_metadata()
```
{{< /tab >}}
{{< tab "input.docx" >}}
{{< tab-text >}}
`input.docx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/adding-metadata/input.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.docx" >}}  
```text
Binary file (DOCX, 14 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/adding-metadata/adding_metadata/output.docx)
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
