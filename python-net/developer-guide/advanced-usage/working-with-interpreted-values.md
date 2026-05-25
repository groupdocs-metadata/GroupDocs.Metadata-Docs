---
id: working-with-interpreted-values
url: metadata/python-net/working-with-interpreted-values
title: Working with interpreted values
weight: 8
description: "Understand and extract human-readable (interpreted) values for metadata properties using GroupDocs.Metadata for Python via .NET."
keywords: interpreted value, enumeration, flag
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
Sometimes it's not obvious what a particular metadata property is supposed to mean. A good example is a numeric flag or enumeration. For instance, here is the description of the EXIF `ExposureProgram` tag taken from the official EXIF specification:

{{< alert style="info" >}}
The class of the program used by the camera to set exposure when the picture is taken. The tag values are as follows.

0 = Not defined

1 = Manual

2 = Normal program

3 = Aperture priority

4 = Shutter priority

5 = Creative program

6 = Action program

7 = Portrait mode

8 = Landscape mode

{{< /alert >}}

As you can see, all modes are represented by numeric values. If you are not familiar with the specification, converting these bare numbers to something meaningful is hard. This is where interpreted values come in — they provide a user-friendly description of the original property value.

To get a full list of properties that have interpreted values for a particular file, use the example below. The original value is available through `prop.value.raw_value` and the human-readable one through `prop.interpreted_value.raw_value`.

{{< tabs "working-with-interpreted-values">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def working_with_interpreted_values():
    with Metadata("input.jpg") as metadata:
        # Keep only properties that expose a human-readable interpreted value
        properties = metadata.find_properties(lambda p: p.interpreted_value is not None)
        for prop in properties:
            print(prop.name)
            print(prop.value.raw_value)              # original (raw) value
            print(prop.interpreted_value.raw_value)  # friendly interpretation
            print()


if __name__ == "__main__":
    working_with_interpreted_values()
```
{{< /tab >}}
{{< tab "input.jpg" >}}
{{< tab-text >}}
`input.jpg` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-interpreted-values/input.jpg) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "working-with-interpreted-values.txt" >}}  
```text
FileFormat
9
Jpeg

ByteOrder
1
BigEndian

XResolution
System.Double[]
[TRUNCATED]
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-interpreted-values/working_with_interpreted_values/working-with-interpreted-values.txt)
{{< /tab >}}
{{< /tabs >}}

From release to release we add interpreters to metadata properties extracted from various formats.

## See also

- [Getting known property descriptors]({{< ref "metadata/python-net/developer-guide/advanced-usage/getting-known-property-descriptors.md" >}})
- [Extracting metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/extracting-metadata.md" >}})
- [Advanced usage]({{< ref "metadata/python-net/developer-guide/advanced-usage/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
