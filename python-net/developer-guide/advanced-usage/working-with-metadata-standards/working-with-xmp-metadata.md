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

The Extensible Metadata Platform (XMP) is an XML-based ISO metadata standard, originally created by Adobe Systems Inc. It defines a data structure, serialization model and basic metadata properties intended to form a unified metadata package that can be embedded into different media formats. The defined XMP data model can be used to store any set of metadata properties. These can be simple name/value pairs, structured values or lists of values. The data can be nested as well.

{{< alert style="info" >}}Please find more information on the XMP standard at https://en.wikipedia.org/wiki/Extensible_Metadata_Platform{{< /alert >}}

## Reading XMP properties

To access XMP metadata in a file of any supported format, GroupDocs.Metadata provides the `IXmp.xmp_package` property. The following are the steps to read XMP metadata:

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains XMP metadata
2.  Extract the XMP metadata package using the `IXmp.xmp_package` property

The following code snippet gets XMP properties of a PNG image and displays them on the screen. 

**advanced_usage.working_with_metadata_standards.xmp.read_xmp_properties**


```python
def run():
    with gm.Metadata(constants.png_with_xmp) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "xmp_package") and root.xmp_package is not None:
            if root.xmp_package.schemes.xmp_basic is not None:
                print(root.xmp_package.schemes.xmp_basic.creator_tool)
                print(root.xmp_package.schemes.xmp_basic.create_date)
                print(root.xmp_package.schemes.xmp_basic.modify_date)
                print(root.xmp_package.schemes.xmp_basic.label)
                print(root.xmp_package.schemes.xmp_basic.nickname)

                # ...

            if root.xmp_package.schemes.dublin_core is not None:
                print(root.xmp_package.schemes.dublin_core.format)
                print(root.xmp_package.schemes.dublin_core.coverage)
                print(root.xmp_package.schemes.dublin_core.identifier)
                print(root.xmp_package.schemes.dublin_core.source)

                # ...

            if root.xmp_package.schemes.photoshop is not None:
                print(root.xmp_package.schemes.photoshop.color_mode)
                print(root.xmp_package.schemes.photoshop.icc_profile)
                print(root.xmp_package.schemes.photoshop.country)
                print(root.xmp_package.schemes.photoshop.city)
                print(root.xmp_package.schemes.photoshop.date_created)

                # ...
```


Here is a non-exhaustive list of supported XMP schemes:

*   XmpBasicJobTicketPackage
*   XmpBasicPackage
*   XmpCameraRawPackage
*   XmpDublinCorePackage
*   XmpDynamicMediaPackage
*   XmpIptcCorePackage
*   XmpIptcExtensionPackage
*   XmpIptcIimPackage
*   XmpMediaManagementPackage
*   XmpPagedTextPackage
*   XmpPdfPackage
*   XmpPhotoshopPackage
*   XmpRightsManagementPackage

{{< alert style="info" >}}GroupDocs.Metadata also provides an API allowing users to work with fully custom XMP schemes/packages.{{< /alert >}}

## Updating XMP properties

The GroupDocs.Metadata API facilitates the user to update XMP metadata in a convenient way - using the `XmpPacketWrapper` class properties. Follow the below steps to update XMP metadata in a file of any supported format.

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains XMP metadata
2.  Extract the XMP metadata package using the `IXmp.xmp_package` property
3.  Assign values to desired XMP properties
4.  [Save]({{< ref "metadata/python-net/developer-guide/advanced-usage/saving-files/_index.md" >}}) the changes

**advanced_usage.working_with_metadata_standards.xmp.update_xmp_properties**


```python
def run():
    with gm.Metadata(constants.gif_with_xmp) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "xmp_package") and root.xmp_package is not None:
            # if there is no such scheme in the XMP package we should create it
            if root.xmp_package.schemes.dublin_core is None:
                root.xmp_package.schemes.dublin_core = gm.standards.xmp.schemes.XmpDublinCorePackage()
            root.xmp_package.schemes.dublin_core.format = "image/gif"
            root.xmp_package.schemes.dublin_core.set_rights("Copyright (C) 2011-2019 GroupDocs. All Rights Reserved")
            root.xmp_package.schemes.dublin_core.set_subject("test")

            if root.xmp_package.schemes.camera_raw is None:
                root.xmp_package.schemes.camera_raw = gm.standards.xmp.schemes.XmpCameraRawPackage()
            root.xmp_package.schemes.camera_raw.shadows = 50
            root.xmp_package.schemes.camera_raw.auto_brightness = True
            root.xmp_package.schemes.camera_raw.auto_exposure = True
            root.xmp_package.schemes.camera_raw.camera_profile = "test"
            root.xmp_package.schemes.camera_raw.exposure = 0.0001

            # If you don't want to keep the old values just replace the whole scheme
            root.xmp_package.schemes.xmp_basic = gm.standards.xmp.schemes.XmpBasicPackage()
            root.xmp_package.schemes.xmp_basic.create_date = date.today()
            root.xmp_package.schemes.xmp_basic.base_url = "https://groupdocs.com"
            root.xmp_package.schemes.xmp_basic.rating = 5
            root.xmp_package.schemes.basic_job_ticket = gm.standards.xmp.schemes.XmpBasicJobTicketPackage()

            # Set a complex type property
            root.xmp_package.schemes.basic_job_ticket.jobs = [
                gm.standards.xmp.XmpJob(
                    id="1",
                    name="test job",
                    url="https://groupdocs.com"
                )
            ]

            # ...

            metadata.save(constants.output_gif)
```


## Adding a custom XMP package

The GroupDocs.Metadata API provides access to a number of commonly used XMP schemes. But it also allows you to create fully custom XMP packages containing user-defined properties. The example below demonstrates how to create and add a custom XMP package to a file.

**advanced_usage.working_with_metadata_standards.xmp.add_custom_xmp_package**


```python
def run():
    with gm.Metadata(constants.input_jpeg) as metadata:
        root = metadata.get_root_package()
        packet = gm.standards.xmp.XmpPacketWrapper()

        custom = gm.standards.xmp.XmpPackage("gd", "https://groupdocs.com")
        custom.set("gd:Copyright", "Copyright (C) 2011-2019 GroupDocs. All Rights Reserved.")
        custom.set("gd:CreationDate", date.today())
        custom.set("gd:Company", gm.standards.xmp.XmpArray.from_array(["Aspose", "GroupDocs"], gm.standards.xmp.XmpArrayType.ORDERED))
        packet.add_package(custom)

        root.xmp_package = packet

        metadata.save(constants.output_jpeg)
```


## Removing XMP metadata

To remove the XMP package from a file just assign `None` to the `IXmp.xmp_package` property. The code sample below shows how to remove XMP metadata from a file.

**advanced_usage.working_with_metadata_standards.xmp.remove_xmp_metadata**


```python
def run():
    with gm.Metadata(constants.jpeg_with_xmp) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "xmp_package"):
            root.xmp_package = None
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


