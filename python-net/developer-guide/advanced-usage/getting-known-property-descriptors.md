---
id: getting-known-property-descriptors
url: metadata/python-net/getting-known-property-descriptors
title: Getting known property descriptors
weight: 7
description: "Extract information about the known properties available in a particular package using GroupDocs.Metadata for Python via .NET."
keywords: property descriptors, known properties, GroupDocs.Metadata, python
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example demonstrates how to extract information about the known properties that can be encountered in a particular package.

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file to examine
2.  Get the collection of property descriptors for the desired metadata package
3.  Iterate through the extracted descriptors

{{< tabs "getting-known-property-descriptors">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def getting_known_property_descriptors():
    with Metadata("input.doc") as metadata:
        root = metadata.get_root_package()
        # Not every package exposes document properties; guard for it
        document_properties = getattr(root, "document_properties", None)
        if document_properties is not None:
            # Each descriptor describes a known property the package supports
            for descriptor in document_properties.know_property_descriptors:
                print(descriptor.name)          # property name
                print(descriptor.type)          # value type
                print(descriptor.access_level)  # read-only / read-write

                # Tags attached to the property (used by the search predicates)
                for tag in descriptor.tags:
                    print(tag)

                print()


if __name__ == "__main__":
    getting_known_property_descriptors()
```
{{< /tab >}}
{{< tab "input.doc" >}}
{{< tab-text >}}
`input.doc` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/getting-known-property-descriptors/input.doc) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "getting-known-property-descriptors.txt" >}}  
```text
Author
1
7
PersonCreator
DocumentPropertyTypeBuiltIn

Bytes
5
7
DesignationMeasure
[TRUNCATED]
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/getting-known-property-descriptors/getting_known_property_descriptors/getting-known-property-descriptors.txt)
{{< /tab >}}
{{< /tabs >}}

{{< alert style="info" >}}
Not all possible properties are present in the `know_property_descriptors` collection. The library provides information on the most frequently used properties only. If there is no descriptor for some property, it is still accessible through the GroupDocs.Metadata search engine in read-only mode.
{{< /alert >}}

## See also

- [Working with interpreted values]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-interpreted-values.md" >}})
- [Extracting metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/extracting-metadata.md" >}})
- [Advanced usage]({{< ref "metadata/python-net/developer-guide/advanced-usage/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
