---
id: extracting-property-values
url: metadata/python-net/extracting-property-values
title: Extracting property values
weight: 2
description: "Learn common ways to read metadata property values and handle their types in GroupDocs.Metadata for Python via .NET."
keywords: extract property values, read metadata values
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
The most common way to get a metadata property value is to inspect its type and read the underlying value accordingly. Every `MetadataProperty` exposes a `value` (a `PropertyValue`) whose `type` tells you the data type and whose `raw_value` returns the underlying Python value.

{{< tabs "extracting-property-values">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.common import MetadataPropertyType


def extract_using_type():
    with Metadata("input.docx") as metadata:
        # Fetch all metadata properties from the file
        properties = metadata.find_properties(lambda p: True)
        for prop in properties:
            # Process string and date/time properties only (as an example)
            if prop.value.type == MetadataPropertyType.STRING:
                print(f"{prop.name} (string): {prop.value.raw_value}")
            elif prop.value.type == MetadataPropertyType.DATE_TIME:
                print(f"{prop.name} (datetime): {prop.value.raw_value}")


if __name__ == "__main__":
    extract_using_type()
```
{{< /tab >}}
{{< tab "input.docx" >}}
{{< tab-text >}}
`input.docx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-properties/extracting-property-values/input.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "extract-using-type.txt" >}}  
```text
MimeType (string): application/vnd.openxmlformats-officedocument.wordprocessingml.document
Extension (string): .docx
Author (string): Prokofjev Igor
Category (string): 
Comments (string): 
Company (string): Profitgroup
ContentStatus (string): 
CreateTime (datetime): 2016-03-22 17:08:00+00:00
Keywords (string): 
LastPrinted (datetime): 0001-01-01 00:00:00
[TRUNCATED]
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-properties/extracting-property-values/extract_using_type/extract-using-type.txt)
{{< /tab >}}
{{< /tabs >}}

If you need to process many supported types generically, inspect `prop.value.raw_value` together with `prop.value.type` and branch accordingly. This lets you handle arrays, numbers, booleans, nested packages, and so on in a single pass.

## See also

- [Exporting metadata properties]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-properties/exporting-metadata-properties.md" >}})
- [Working with metadata properties]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-properties/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
