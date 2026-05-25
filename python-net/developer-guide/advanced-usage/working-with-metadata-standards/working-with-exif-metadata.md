---
id: working-with-exif-metadata
url: metadata/python-net/working-with-exif-metadata
title: Working with EXIF metadata
weight: 1
description: "Access, read, update, and remove EXIF metadata using GroupDocs.Metadata for Python via .NET."
keywords: EXIF metadata
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
## What is EXIF?

According to the specification, EXIF (Exchangeable image file format) is a standard that specifies the formats for images, sound, and tags used by digital still cameras and other systems that handle the image and sound files they record. Despite the confusing name, EXIF is just a metadata standard: it defines a way to store metadata properties in a variety of well-known image and audio formats. The EXIF tag structure is borrowed from TIFF files and declares tags that store technical details such as geolocation, the camera owner's name, camera settings, and so on.

{{< alert style="info" >}}
Please refer to the following article for more information on the standard: https://en.wikipedia.org/wiki/Exif
{{< /alert >}}

## Reading basic EXIF properties

To access EXIF metadata in a file of any supported format, GroupDocs.Metadata provides the `exif_package` property on the root package.

{{< tabs "exif-read-basic">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def read_basic_exif_properties():
    with Metadata("exif.tiff") as metadata:
        root = metadata.get_root_package()
        # The EXIF package is exposed on the root; it may be absent
        exif = getattr(root, "exif_package", None)
        if exif is not None:
            # Top-level EXIF tags
            print(exif.artist)
            print(exif.copyright)
            print(exif.image_description)
            print(exif.make)
            print(exif.model)
            print(exif.software)

            # The EXIF IFD sub-package holds camera/exposure details
            print(exif.exif_ifd_package.body_serial_number)
            print(exif.exif_ifd_package.camera_owner_name)
            print(exif.exif_ifd_package.user_comment)

            # The GPS sub-package holds geolocation tags
            print(exif.gps_package.altitude)
            print(exif.gps_package.latitude_ref)
            print(exif.gps_package.longitude_ref)


if __name__ == "__main__":
    read_basic_exif_properties()
```
{{< /tab >}}
{{< tab "exif.tiff" >}}
{{< tab-text >}}
`exif.tiff` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/exif.tiff) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "read-basic-exif-properties.txt" >}}  
```text
None
None
None
None
None
Adobe Photoshop CS6 (Windows)
None
None
test comment
1/5
[TRUNCATED]
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/read_basic_exif_properties/read-basic-exif-properties.txt)
{{< /tab >}}
{{< /tabs >}}

## Reading all EXIF tags

In some cases it's necessary to read all EXIF tags from a file, including custom ones. The API provides direct access to the EXIF tags on different levels.

{{< tabs "exif-read-tags">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def read_exif_tags():
    with Metadata("exif.jpg") as metadata:
        root = metadata.get_root_package()
        exif = getattr(root, "exif_package", None)
        if exif is not None:
            # to_list() yields every raw tag (id + value) at each level
            for tag in exif.to_list():
                print(f"{tag.tag_id} = {tag.value}")

            # ...including the EXIF IFD sub-package
            for tag in exif.exif_ifd_package.to_list():
                print(f"{tag.tag_id} = {tag.value}")

            # ...and the GPS sub-package
            for tag in exif.gps_package.to_list():
                print(f"{tag.tag_id} = {tag.value}")


if __name__ == "__main__":
    read_exif_tags()
```
{{< /tab >}}
{{< tab "exif.jpg" >}}
{{< tab-text >}}
`exif.jpg` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/exif.jpg) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "read-exif-tags.txt" >}}  
```text
282 = System.Double[]
283 = System.Double[]
296 = System.Int32[]
531 = System.Int32[]
34665 = System.Int64[]
34853 = System.Int64[]
42032 = James B.
36864 = System.Byte[]
37121 = System.Byte[]
40961 = System.Int32[]
[TRUNCATED]
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/read_exif_tags/read-exif-tags.txt)
{{< /tab >}}
{{< /tabs >}}

## Reading a specific EXIF tag

The API supports reading a specific EXIF tag through an indexer that takes a `TiffTagID`.

{{< tabs "exif-read-specific">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.formats.image import TiffTagID


def read_specific_exif_tag():
    with Metadata("exif.tiff") as metadata:
        root = metadata.get_root_package()
        exif = getattr(root, "exif_package", None)
        if exif is not None:
            # Index the package by TiffTagID to read one specific tag
            software = exif[TiffTagID.SOFTWARE]
            if software is not None:
                print(f"Software: {software.value}")

            # The same indexer works on the EXIF IFD sub-package
            comment = exif.exif_ifd_package[TiffTagID.USER_COMMENT]
            if comment is not None:
                print(f"Comment: {comment.interpreted_value}")


if __name__ == "__main__":
    read_specific_exif_tag()
```
{{< /tab >}}
{{< tab "exif.tiff" >}}
{{< tab-text >}}
`exif.tiff` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/exif.tiff) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "read-specific-exif-tag.txt" >}}  
```text
Software: Adobe Photoshop CS6 (Windows)
Comment: test comment
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/read_specific_exif_tag/read-specific-exif-tag.txt)
{{< /tab >}}
{{< /tabs >}}

## Updating EXIF properties

Update EXIF metadata in a convenient way using the `ExifPackage` properties. If the package is missing, create a new one.

{{< tabs "exif-update">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.standards.exif import ExifPackage


def update_exif_properties():
    with Metadata("input.jpg") as metadata:
        root = metadata.get_root_package()
        # Create an EXIF package if the image doesn't have one yet
        if getattr(root, "exif_package", None) is None:
            root.exif_package = ExifPackage()

        # Assign top-level EXIF properties
        root.exif_package.copyright = "Copyright (C) 2026 GroupDocs. All Rights Reserved."
        root.exif_package.image_description = "test image"
        root.exif_package.software = "GroupDocs.Metadata"

        # Assign properties on the EXIF IFD sub-package
        root.exif_package.exif_ifd_package.body_serial_number = "test"
        root.exif_package.exif_ifd_package.camera_owner_name = "GroupDocs"
        root.exif_package.exif_ifd_package.user_comment = "test comment"

        # Persist the changes
        metadata.save("output.jpg")


if __name__ == "__main__":
    update_exif_properties()
```
{{< /tab >}}
{{< tab "input.jpg" >}}
{{< tab-text >}}
`input.jpg` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/input.jpg) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.jpg" >}}  
```text
Binary file (JPG, 795 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/update_exif_properties/output.jpg)
{{< /tab >}}
{{< /tabs >}}

## Adding or updating custom EXIF tags

The API allows adding or updating custom tags in an EXIF package using the typed TIFF tag classes.

{{< tabs "exif-set-custom">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.formats.image import TiffAsciiTag, TiffTagID
from groupdocs.metadata.standards.exif import ExifPackage


def set_custom_exif_tag():
    with Metadata("exif.tiff") as metadata:
        root = metadata.get_root_package()
        if getattr(root, "exif_package", None) is None:
            root.exif_package = ExifPackage()

        # Add known properties using typed TIFF tags
        root.exif_package.set(TiffAsciiTag(TiffTagID.ARTIST, "test artist"))
        root.exif_package.set(TiffAsciiTag(TiffTagID.SOFTWARE, "GroupDocs.Metadata"))

        metadata.save("output.tiff")


if __name__ == "__main__":
    set_custom_exif_tag()
```
{{< /tab >}}
{{< tab "exif.tiff" >}}
{{< tab-text >}}
`exif.tiff` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/exif.tiff) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.tiff" >}}  
```text
Binary file (TIFF, 135 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/set_custom_exif_tag/output.tiff)
{{< /tab >}}
{{< /tabs >}}

The following typed tag classes are available in `groupdocs.metadata.formats.image`: `TiffAsciiTag`, `TiffByteTag`, `TiffDoubleTag`, `TiffFloatTag`, `TiffLongTag`, `TiffRationalTag`, `TiffSByteTag`, `TiffShortTag`, `TiffSLongTag`, `TiffSRationalTag`, `TiffSShortTag`, `TiffUndefinedTag`.

## Removing EXIF metadata

To remove the EXIF package from a file, assign `None` to the `exif_package` property.

{{< tabs "exif-remove">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def remove_exif_metadata():
    with Metadata("exif.jpg") as metadata:
        root = metadata.get_root_package()
        # Assigning None drops the entire EXIF package
        root.exif_package = None
        metadata.save("output.jpg")


if __name__ == "__main__":
    remove_exif_metadata()
```
{{< /tab >}}
{{< tab "exif.jpg" >}}
{{< tab-text >}}
`exif.jpg` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/exif.jpg) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.jpg" >}}  
```text
Binary file (JPG, 795 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata/remove_exif_metadata/output.jpg)
{{< /tab >}}
{{< /tabs >}}

## See also

- [Working with IPTC IIM metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata.md" >}})
- [Working with XMP metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata.md" >}})
- [Working with metadata standards]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
