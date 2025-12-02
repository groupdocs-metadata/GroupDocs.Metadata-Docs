---
id: find-metadata-properties
url: metadata/python-net/find-metadata-properties
title: Find metadata properties
weight: 2
description: Using the GroupDocs.Metadata for Python via .NET you can easily search metadata and extract desired metadata properties from PDF, DOCX, PPTX, XLSX, images, audio, video and many other files of different types in your Java solution.
keywords: search metadata, metadata, PDF, DOCX, XLSX, pptx
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
## Use tags to find most common metadata properties

To make manipulating metadata easier we attach specific tags to the most commonly used metadata properties extracted from a file. Some metadata standards can have quite a complex structure. Moreover, in most cases, one image, video or document contains more than one metadata packages. Using tags you can search for desirable properties with a few lines of code without even knowing the exact format of the loaded file.

The code sample below demonstrates how to search for specific metadata properties using tags:

1.  **Load** a file to examine
2.  Make up a predicate checking that a specific tag is assigned to a property (alternatively you can use a combination of tags)
3.  Pass the predicate to the **findProperties** method
4.  Iterate through the found properties

**basic\_usage.FindMetadataProperties**


```python
# Fetch all the properties satisfying the predicate:
    # property contains the name of the last document editor OR the date/time the document was last modified
with gm.Metadata(constants.input_pptx) as metadata:
        specification = gm.search.ContainsTagSpecification(gm.tagging.Tags.person.editor).either(gm.search.ContainsTagSpecification(gm.tagging.Tags.time.modified))
        properties = metadata.find_properties(specification)
        for property in properties:
             print(f"Property name: {property.name}, Property value: {property.value}")
```


As a result, we obtain all metadata properties containing the name of the person last edited the document and all properties that store the date/time the document was last edited.

For more information on supported features of the GroupDocs.Metadata search engine please refer to the following articles:

*   [Extracting metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/extracting-metadata.md" >}})
*   [Removing metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/removing-metadata.md" >}})
*   [Adding metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/adding-metadata.md" >}})

## More resources

### Advanced usage topics

To learn more about library features and get familiar how to manage metadata and more, please refer to the[advanced usage section]({{< ref "metadata/python-net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)
    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)

*   [GroupDocs.Metadata for Node.js via Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Node.js-via-Java)

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)
    

### Free online document metadata management App

Along with a full featured Java library we provide simple, but powerful free Apps.

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
