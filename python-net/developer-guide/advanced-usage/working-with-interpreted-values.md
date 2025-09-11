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
Sometimes it's not really obvious what a particular metadata property is supposed to mean. A good example of such vague property is a numeric flag or enumeration. For example, here is the description of the EXIF ExposureProgram tag taken from the official EXIF specification:

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

As you can see, all modes are represented by numeric values. If you are not familiar with the specification, you will get a hard time converting these bare numbers to something meaningful. This is where interpreted values come into play. They provide a user-friendly description of the original property value. The code snippet below demonstrates how to extract the ExposureProgram property and display its original value and interpreted value.


```python
def run_single_file():
    with gm.Metadata(r"D:\\input.heic") as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "exif_package") and root.exif_package is not None:
            prop = root.exif_package.exif_ifd_package[TiffTagID.EXPOSURE_PROGRAM]
            if prop is not None:
                print(prop.tag_value[0])  # e.g., 2
                print(prop.interpreted_value)  # e.g., Normal program
```


From release to release, we add interpreters to metadata properties extracted from various formats. To get a full list of properties having interpreted values for a particular file please use the below example:

**advanced_usage.working_with_interpreted_values**


```python
def run():
    files = os.listdir(constants.input_path)
    for file in files:
        full = constants.input_path + file
        with gm.Metadata(full) as metadata:
            if metadata.file_format != gm.common.FileFormat.UNKNOWN and metadata.get_document_info().is_encrypted != True:
                print()
                print(full)
                properties = metadata.find_properties(lambda p: p.interpreted_value is not None)
                for prop in properties:
                    print(prop.name)
                    print(prop.value.raw_value)
                    print(prop.interpreted_value.raw_value)
```


## More resources
### GitHub examples
You may easily run the code above and see the feature in action in our GitHub examples:
*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)    
*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App
Along with full featured Python via .NET library we provide simple, but powerful free Apps.
You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).


