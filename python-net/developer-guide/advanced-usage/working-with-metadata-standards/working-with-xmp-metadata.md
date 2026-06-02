---
id: working-with-xmp-metadata
url: metadata/python-net/working-with-xmp-metadata
title: Working with XMP metadata
weight: 3
description: "Access, read, update, add custom packages, and remove XMP metadata using GroupDocs.Metadata for Python via .NET."
keywords: XMP, XMP metadata
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
## What is XMP?

The Extensible Metadata Platform (XMP) is an XML-based ISO metadata standard, originally created by Adobe Systems. It defines a data structure, a serialization model, and basic metadata properties that form a unified metadata package which can be embedded into different media formats. The XMP data model can store any set of metadata properties — simple name/value pairs, structured values, or lists of values, nested arbitrarily.

{{< alert style="info" >}}Find more information on the XMP standard at https://en.wikipedia.org/wiki/Extensible_Metadata_Platform{{< /alert >}}

## Reading XMP properties

To access XMP metadata in a file of any supported format, GroupDocs.Metadata provides the `xmp_package` property on the root package. Standard schemes are reachable through `xmp_package.schemes`.

{{< tabs "xmp-read">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def read_xmp_properties():
    with Metadata("xmp.png") as metadata:
        root = metadata.get_root_package()
        xmp = getattr(root, "xmp_package", None)
        if xmp is not None:
            # Standard schemes are reachable through xmp.schemes;
            # each may be absent, so guard before reading.
            if xmp.schemes.xmp_basic is not None:
                print(xmp.schemes.xmp_basic.creator_tool)
                print(xmp.schemes.xmp_basic.create_date)
                print(xmp.schemes.xmp_basic.modify_date)

            if xmp.schemes.dublin_core is not None:
                print(xmp.schemes.dublin_core.format)
                print(xmp.schemes.dublin_core.coverage)
                print(xmp.schemes.dublin_core.identifier)

            if xmp.schemes.photoshop is not None:
                print(xmp.schemes.photoshop.color_mode)
                print(xmp.schemes.photoshop.city)
                print(xmp.schemes.photoshop.date_created)


if __name__ == "__main__":
    read_xmp_properties()
```
{{< /tab >}}
{{< tab "xmp.png" >}}
{{< tab-text >}}
`xmp.png` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata/xmp.png) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "read-xmp-properties.txt" >}}  
```text
Adobe Photoshop CC 2017 (Windows)
2018-09-05 07:28:22+03:00
2018-09-05 07:37:28+03:00
image/png
None
None
3
None
None
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata/read_xmp_properties/read-xmp-properties.txt)
{{< /tab >}}
{{< /tabs >}}

Here is a non-exhaustive list of supported XMP schemes (in `groupdocs.metadata.standards.xmp.schemes`): `XmpBasicJobTicketPackage`, `XmpBasicPackage`, `XmpCameraRawPackage`, `XmpDublinCorePackage`, `XmpDynamicMediaPackage`, `XmpIptcCorePackage`, `XmpIptcExtensionPackage`, `XmpIptcIimPackage`, `XmpMediaManagementPackage`, `XmpPagedTextPackage`, `XmpPdfPackage`, `XmpPhotoshopPackage`, `XmpRightsManagementPackage`.

## Updating XMP properties

Update XMP metadata through the scheme packages. Create a scheme if it is missing.

{{< tabs "xmp-update">}}
{{< tab "Python" >}}
```python
from datetime import date

from groupdocs.metadata import Metadata
from groupdocs.metadata.standards.xmp.schemes import XmpBasicPackage, XmpCameraRawPackage, XmpDublinCorePackage


def update_xmp_properties():
    with Metadata("xmp.gif") as metadata:
        root = metadata.get_root_package()
        xmp = getattr(root, "xmp_package", None)
        if xmp is not None:
            if xmp.schemes.dublin_core is None:
                xmp.schemes.dublin_core = XmpDublinCorePackage()
            xmp.schemes.dublin_core.format = "image/gif"

            if xmp.schemes.camera_raw is None:
                xmp.schemes.camera_raw = XmpCameraRawPackage()
            xmp.schemes.camera_raw.shadows = 50
            xmp.schemes.camera_raw.camera_profile = "test"

            # Replace the whole scheme to drop old values
            xmp.schemes.xmp_basic = XmpBasicPackage()
            xmp.schemes.xmp_basic.create_date = date.today()
            xmp.schemes.xmp_basic.rating = 5

            metadata.save("output.gif")


if __name__ == "__main__":
    update_xmp_properties()
```
{{< /tab >}}
{{< tab "xmp.gif" >}}
{{< tab-text >}}
`xmp.gif` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata/xmp.gif) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.gif" >}}  
```text
Binary file (GIF, 150 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata/update_xmp_properties/output.gif)
{{< /tab >}}
{{< /tabs >}}

## Adding a custom XMP package

Besides the predefined schemes, you can create fully custom XMP packages with user-defined properties.

{{< tabs "xmp-add-custom">}}
{{< tab "Python" >}}
```python
from datetime import date

from groupdocs.metadata import Metadata
from groupdocs.metadata.standards.xmp import XmpArray, XmpArrayType, XmpPackage, XmpPacketWrapper


def add_custom_xmp_package():
    with Metadata("input.jpg") as metadata:
        root = metadata.get_root_package()
        packet = XmpPacketWrapper()

        custom = XmpPackage("gd", "https://groupdocs.com")
        custom.set("gd:Copyright", "Copyright (C) 2026 GroupDocs. All Rights Reserved.")
        custom.set("gd:CreationDate", date.today())
        custom.set("gd:Company", XmpArray.from_(["Aspose", "GroupDocs"], XmpArrayType.ORDERED))
        packet.add_package(custom)

        root.xmp_package = packet
        metadata.save("output.jpg")


if __name__ == "__main__":
    add_custom_xmp_package()
```
{{< /tab >}}
{{< tab "input.jpg" >}}
{{< tab-text >}}
`input.jpg` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata/input.jpg) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.jpg" >}}  
```text
Binary file (JPG, 885 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata/add_custom_xmp_package/output.jpg)
{{< /tab >}}
{{< /tabs >}}

## Removing XMP metadata

To remove the XMP package from a file, assign `None` to the `xmp_package` property.

{{< tabs "xmp-remove">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def remove_xmp_metadata():
    with Metadata("xmp.jpg") as metadata:
        root = metadata.get_root_package()
        # Assigning None drops the entire XMP packet
        root.xmp_package = None
        metadata.save("output.jpg")


if __name__ == "__main__":
    remove_xmp_metadata()
```
{{< /tab >}}
{{< tab "xmp.jpg" >}}
{{< tab-text >}}
`xmp.jpg` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata/xmp.jpg) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.jpg" >}}  
```text
Binary file (JPG, 174 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata/remove_xmp_metadata/output.jpg)
{{< /tab >}}
{{< /tabs >}}

## See also

- [Working with EXIF metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata.md" >}})
- [Working with IPTC IIM metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata.md" >}})
- [Working with metadata standards]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
