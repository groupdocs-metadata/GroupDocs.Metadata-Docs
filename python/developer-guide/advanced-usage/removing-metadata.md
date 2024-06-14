---
id: removing-metadata
url: metadata/python-net/removing-metadata
title: Removing metadata
weight: 3
description: ""
keywords: 
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
Not all metadata properties extracted from a file are marked with tags. Some file formats and metadata standards allow adding fully custom properties that can't be properly tagged by the library since their purpose is not clearly defined in the appropriate format/standard specification. In such cases, you can use the name of the property to locate and remove it. The following example demonstrates some advanced usage scenarios of the GroupDocs.Metadata search engine allowing to remove metadata properties.

1.  **Load** a file to be modified
2.  Pass a search predicate to the **removeProperties** method.
3.  Check the number of properties that were actually removed
4.  **Save** the changes

**advanced\_usage.RemovingMetadata**

{{< tabs "example1">}}
{{< tab "Python" >}}
```python
def run():
    files = os.listdir(constants.input_path)
    for file in files:
        with gm.Metadata(constants.input_path+file) as metadata:
            if metadata.file_format != gm.common.FileFormat.UNKNOWN and metadata.get_document_info().is_encrypted != True:
                print()
                print(file)

                specification = gm.search.FallsIntoCategorySpecification(gm.tagging.Tags.content)
                affected = metadata.remove_properties(specification)
                print(f"Affected properties: {affected}")
                metadata.save(constants.output_path + "output" + pathlib.Path(file).suffix)

```
{{< /tab >}}
{{< /tabs >}}

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
