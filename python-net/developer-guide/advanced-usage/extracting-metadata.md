---
id: extracting-metadata
url: metadata/python-net/extracting-metadata
title: Extracting metadata
weight: 2
description: "Extract the metadata properties you need from files of different types with GroupDocs.Metadata for Python via .NET using tags, categories and property attributes."
keywords: extract metadata, tags, categories, GroupDocs.Metadata, python
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
Using GroupDocs.Metadata you can extract the metadata properties you need from files of different types. You don't have to worry about the exact file format or the metadata standards it uses — the same code works for all supported formats. Most commonly used metadata properties are marked with tags, and all tags are grouped into categories that make it easier to find the one you need. The code sample below demonstrates how to use tags, categories and other property attributes.

1.  **Load** a file to search for metadata properties
2.  Build a predicate to examine every extracted metadata property
3.  Pass the predicate to the **find_properties** method
4.  Iterate through the found properties

{{< tabs "extracting-metadata">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.tagging import Tags


def extracting_metadata():
    with Metadata("input.docx") as metadata:
        # Find every property whose tags fall into the "content" category
        properties = metadata.find_properties(
            lambda p: any(tag.category == Tags.content for tag in p.tags)
        )
        for prop in properties:
            print(f"Property name: {prop.name}, Property value: {prop.value}")


if __name__ == "__main__":
    extracting_metadata()
```
{{< /tab >}}
{{< tab "input.docx" >}}
{{< tab-text >}}
`input.docx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/extracting-metadata/input.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extracting-metadata.txt" >}}  
```text
Property name: FileFormat, Property value: 3
Property name: MimeType, Property value: application/vnd.openxmlformats-officedocument.wordprocessingml.document
Property name: WordProcessingFileFormat, Property value: 3
Property name: Category, Property value: 
Property name: Comments, Property value: 
Property name: ContentStatus, Property value: 
Property name: Keywords, Property value: 
Property name: RevisionNumber, Property value: 9
Property name: Subject, Property value: 
Property name: Title
[TRUNCATED]
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/extracting-metadata/extracting_metadata/extracting-metadata.txt)
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
