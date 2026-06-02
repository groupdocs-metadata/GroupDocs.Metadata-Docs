---
id: find-metadata-properties
url: metadata/python-net/find-metadata-properties
title: Find metadata properties
weight: 2
description: With GroupDocs.Metadata for Python via .NET you can search metadata and extract the properties you need from PDF, DOCX, PPTX, XLSX, images, audio, video and many other file types using simple Python predicates.
keywords: search metadata, metadata, PDF, DOCX, XLSX, pptx
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
## Use tags to find most common metadata properties

To make manipulating metadata easier, GroupDocs.Metadata attaches specific tags to the most commonly used metadata properties extracted from a file. Some metadata standards can have quite a complex structure. Moreover, in most cases one image, video or document contains more than one metadata package. Using tags you can search for the properties you want with a few lines of code without even knowing the exact format of the loaded file.

In Python you search by passing a **predicate** — any `lambda` (or function) that takes a single `MetadataProperty` and returns `True`/`False` — to the `find_properties` method. The `groupdocs.metadata.tagging.Tags` catalog gives you the tags to test against.

The code sample below demonstrates how to search for specific metadata properties using tags:

1.  **Load** a file to examine
2.  Build a predicate that checks a property carries a specific tag (or a combination of tags)
3.  Pass the predicate to the **find_properties** method
4.  Iterate through the found properties

{{< tabs "find-metadata-properties">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.tagging import Tags


def find_metadata_properties():
    # Fetch all the properties satisfying the predicate:
    # the property carries the "last editor" tag OR the "modified date/time" tag
    with Metadata("input.pptx") as metadata:
        properties = metadata.find_properties(
            lambda p: Tags.person.editor in list(p.tags)
            or Tags.time.modified in list(p.tags)
        )
        for prop in properties:
            print(f"Property name: {prop.name}, Property value: {prop.value}")


if __name__ == "__main__":
    find_metadata_properties()
```
{{< /tab >}}
{{< tab "input.pptx" >}}
{{< tab-text >}}
`input.pptx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/basic-usage/find-metadata-properties/input.pptx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "find-metadata-properties.txt" >}}  
```text
Property name: lastsavedby, Property value: tttlll@sibmail.com
Property name: lastsavedtime, Property value: 10/28/2019 09:59:47
```
[Download full output](/metadata/python-net/_output_files/developer-guide/basic-usage/find-metadata-properties/find_metadata_properties/find-metadata-properties.txt)
{{< /tab >}}
{{< /tabs >}}

As a result, we obtain all metadata properties that hold the name of the person who last edited the document and all properties that store the date/time the document was last edited.

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
