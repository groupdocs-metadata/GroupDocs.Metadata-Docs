---
id: extracting-metadata
url: metadata/python-net/extracting-metadata
title: Extracting metadata
weight: 2
description: ""
keywords: 
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
Using the GroupDocs.Metadata search engine you can extract desired metadata properties from files of different types. You don't need to worry about the exact file format and metadata standards it can deal with. The same code will work for all supported formats in the same way. Most commonly used metadata properties are marked with tags that allow searching them across all supported files in various metadata packages. All tags defined in GroupDocs.Metadata are divided into categories that make it easier to find a required tag. The code sample below demonstrates some advanced usage of tags, categories and other attributes of metadata properties.

1.  **Load** a file to be searched for metadata properties
2.  Make up a predicate to examine all extracted metadata properties
3.  Pass the predicate to the **findProperties** method
4.  Iterate through the found properties

**advanced\_usage.ExtractingMetadata**


```python
def run():
    files = os.listdir(constants.input_path)
    for file in files:
        with gm.Metadata(constants.input_path+file) as metadata:
            if metadata.file_format != gm.common.FileFormat.UNKNOWN and metadata.get_document_info().is_encrypted != True:
                print()
                print(file)

                specification = gm.search.FallsIntoCategorySpecification(gm.tagging.Tags.content)
                properties = metadata.find_properties(specification)
                for property in properties:
                    print(f"Property name: {property.name}, Property value: {property.value}")
```


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
