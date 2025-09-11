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

According to the specification, EXIF (Exchangeable image file format) is a standard that specifies the formats to be used for images, sound and tags in digital still cameras and in other systems handling the image and sound files recorded by digital still cameras. Despite the confusing definition and name of the format, EXIF is just a metadata standard. In fact, it simply defines a way to store metadata properties in a variety of well-known image and audio formats. The EXIF tag structure is borrowed from TIFF files. The specification declares a set of tags intended to store technical details such as the geolocation of the place where a picture was taken, the name of the camera owner, camera settings, etc. 

{{< alert style="info" >}}
Please refer to the following article to get more information on the standard: https://en.wikipedia.org/wiki/Exif
{{< /alert >}}

## Reading basic EXIF properties

To access EXIF metadata in a file of any supported format, GroupDocs.Metadata provides the `IExif.exif_package` property. The following are the steps to read EXIF metadata:

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains EXIF metadata
2.  Extract the EXIF metadata package using the `IExif.exif_package` property

The following code snippet gets EXIF properties of a TIFF image and displays them on the screen. 

**advanced_usage.working_with_metadata_standards.exif.read_basic_exif_properties**


```python
def run():
    with gm.Metadata(constants.tiff_with_exif) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "exif_package") and root.exif_package is not None:
            print(root.exif_package.artist)
            print(root.exif_package.copyright)
            print(root.exif_package.image_description)
            print(root.exif_package.make)
            print(root.exif_package.model)
            print(root.exif_package.software)
            print(root.exif_package.image_width)
            print(root.exif_package.image_length)

            # ...

            print(root.exif_package.exif_ifd_package.body_serial_number)
            print(root.exif_package.exif_ifd_package.camera_owner_name)
            print(root.exif_package.exif_ifd_package.user_comment)

            # ...

            print(root.exif_package.gps_package.altitude)
            print(root.exif_package.gps_package.latitude_ref)
            print(root.exif_package.gps_package.longitude_ref)
```


## Reading all EXIF tags

In some cases, it's necessary to read all EXIF properties from a file, including custom ones. To achieve this the GroupDocs.Metadata API provides direct access to the EXIF tags extracted from a file.

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains EXIF metadata
2.  Extract the EXIF metadata package using the `IExif.exif_package` property
3.  Iterate through all EXIF tags on different levels

**advanced_usage.working_with_metadata_standards.exif.read_exif_tags**


```python
def run():
    with gm.Metadata(constants.jpeg_with_exif) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "exif_package") and root.exif_package is not None:
            pattern = "{0} = {1}"

            for tag in root.exif_package.to_list():
                print(pattern.format(tag.tag_id, tag.value))

            for tag in root.exif_package.exif_ifd_package.to_list():
                print(pattern.format(tag.tag_id, tag.value))

            for tag in root.exif_package.gps_package.to_list():
                print(pattern.format(tag.tag_id, tag.value))
```


## Reading a specific EXIF tag

The GroupDocs.Metadata API also supports reading specific EXIF tags using an indexer. Follow below-mentioned steps to read a specific EXIF tag.

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains EXIF metadata
2.  Extract the EXIF metadata package using the `IExif.exif_package` property
3.  Get a specific tag using the `ExifPackage` class indexer

**advanced_usage.working_with_metadata_standards.exif.read_specific_exif_tag**


```python
def run():
    with gm.Metadata(constants.tiff_with_exif) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "exif_package") and root.exif_package is not None:
            software = root.exif_package[TiffTagID.SOFTWARE]
            if software is not None:
                print(f"Software: {software.value}")

            comment = root.exif_package.exif_ifd_package[TiffTagID.USER_COMMENT]
            if comment is not None:
                print(f"Comment: {comment.interpreted_value}")
```


## Updating EXIF properties

The GroupDocs.Metadata API facilitates the user to update EXIF metadata in a convenient way - using the `ExifPackage` class properties. Follow the below steps to update EXIF metadata in a file of any supported format.

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains EXIF metadata
2.  Extract the EXIF metadata package using the `IExif.exif_package` property
3.  Assign values to desired EXIF properties
4.  [Save]({{< ref "metadata/python-net/developer-guide/advanced-usage/saving-files/_index.md" >}}) the changes

**advanced_usage.working_with_metadata_standards.exif.update_exif_properties**


```python
def run():
    with gm.Metadata(constants.input_jpeg) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "exif_package") is False or root.exif_package is None:
            root.exif_package = gm.standards.exif.ExifPackage()

        root.exif_package.copyright = "Copyright (C) 2011-2019 GroupDocs. All Rights Reserved."
        root.exif_package.image_description = "test image"
        root.exif_package.software = "GroupDocs.Metadata"

        # ...

        root.exif_package.exif_ifd_package.body_serial_number = "test"
        root.exif_package.exif_ifd_package.camera_owner_name = "GroupDocs"
        root.exif_package.exif_ifd_package.user_comment = "test comment"

        # ...

        metadata.save(constants.output_jpeg)
```


## Adding or updating custom EXIF tags

The GroupDocs.Metadata API allows adding or updating custom tags in an EXIF package.

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains EXIF metadata
2.  Extract the EXIF metadata package using the `IExif.exif_package` property
3.  Set the EXIF package if it's missing
4.  Add any number of custom tags to the package
5.  [Save]({{< ref "metadata/python-net/developer-guide/advanced-usage/saving-files/_index.md" >}}) the changes

**advanced_usage.working_with_metadata_standards.exif.set_custom_exif_tag**


```python
def run():
    with gm.Metadata(constants.tiff_with_exif) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "exif_package") is False or root.exif_package is None:
            root.exif_package = gm.standards.exif.ExifPackage()

        # Add a known property
        root.exif_package.set(gm.formats.image.TiffAsciiTag(TiffTagID.ARTIST, "test artist"))

        # Add a fully custom property (which is not described in the EXIF specification).
        # Please note that the chosen ID may intersect with the IDs used by some third party tools.
        root.exif_package.set(gm.formats.image.TiffAsciiTag(TiffTagID(65523), "custom"))

        metadata.save(constants.output_tiff)
```


Here is a full list of tags that can be added to an EXIF package:

*   `TiffAsciiTag`
*   `TiffByteTag`
*   `TiffDoubleTag`
*   `TiffFloatTag`
*   `TiffLongTag`
*   `TiffRationalTag`
*   `TiffSByteTag`
*   `TiffShortTag`
*   `TiffSLongTag`
*   `TiffSRationalTag`
*   `TiffSShortTag`
*   `TiffUndefinedTag`

## Removing EXIF metadata

To remove the EXIF package from a file just assign `None` to the `IExif.exif_package` property. The code sample below shows how to remove EXIF metadata from a file.

**advanced_usage.working_with_metadata_standards.exif.remove_exif_metadata**


```python
def run():
    with gm.Metadata(constants.jpeg_with_exif) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "exif_package"):
            root.exif_package = None
            metadata.save(constants.output_jpeg)
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


