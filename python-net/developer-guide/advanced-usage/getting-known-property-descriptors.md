---
id: getting-known-property-descriptors
url: metadata/python-net/getting-known-property-descriptors
title: Getting known property descriptors
weight: 7
description: "Extract information about known properties available in a particular package using GroupDocs.Metadata for Python via .NET."
keywords: extract information
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example demonstrates how to extract information about known properties that can be encountered in a particular package.

2.  Get a collection of property descriptors for the desired metadata package
3.  Iterate through the extracted descriptors

**advanced_usage.getting_known_property_descriptors**


```python
def run():
    with gm.Metadata(constants.input_doc) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "document_properties") and root.document_properties is not None:
            for descriptor in root.document_properties.know_property_descriptors:
                print(descriptor.name)
                print(descriptor.type)
                print(descriptor.access_level)

                for tag in descriptor.tags:
                    print(tag)

                print()
```


{{< alert style="info" >}}
Not all possible properties are presented in the KnowPropertyDescriptors collection. The library provides information on the most frequently used properties only. If there is no descriptor for some property it is still accessible through the GroupDocs.Metadata search engine in read-only mode.
{{< /alert >}}

## More resources
### GitHub examples
You may easily run the code above and see the feature in action in our GitHub examples:
*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)    
*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App
Along with full featured Python via .NET library we provide simple, but powerful free Apps.
You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).


