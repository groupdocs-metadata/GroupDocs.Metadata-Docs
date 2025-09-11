---
id: set-metadata-properties
url: metadata/python-net/set-metadata-properties
title: Set metadata properties
weight: 4
description:  The SetProperties method is used to update or add metadata. You can easily add metadata to photos, pdfs or you can update or add data to mp3 files.
keywords: add metadata to pdf,add metadata to photos,add metadata to mp3, add metadata
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
## Update or add metadata properties satisfying a predicate

The **setProperties** method used in this code sample actually combines two operations: add and update. If an existing property satisfies the specified predicate its value is updated. If there is a known property missing in a metadata package that satisfies the predicate it is added to the appropriate package.

The code snippet below demonstrates a basic usage scenario of the **setProperties** method.

1.  **Open** a file to update
2.  Specify a predicate that will be used to add/update metadata properties
3.  Specify a value you would like to add to existing metadata packages in the file
4.  Check the actual number of added/updated properties
5.  **Save** the changes

**basic\_usage.SetMetadataProperties**


```python
with gm.Metadata(constants.input_vsdx) as metadata:
        specification = gm.search.ContainsTagSpecification(gm.tagging.Tags.time.created).either(gm.search.ContainsTagSpecification(gm.tagging.Tags.time.modified))
        now = datetime.now()
        property_value = gm.common.PropertyValue(now)
        affected = metadata.set_properties(specification, property_value)
        print(f"Properties set: {affected}")
        metadata.save(constants.output_vsdx)
```


As a result, we update all existing metadata properties containing the date the document was created/updated. If a metadata package doesn't contain such properties but they are meant to be in its structure they are added.

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
