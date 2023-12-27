---
id: working-with-exif-metadata
url: metadata/nodejs-java/working-with-exif-metadata
title: Working with EXIF metadata
weight: 1
description: "This article explains how to access EXIF metadata in a file of any supported format, GroupDocs.Metadata for Java provides the IExif.getExifPackage method."
keywords: EXIF metadata
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
## What is EXIF?

According to the [specification](https://www.exif.org/Exif2-2.PDF), EXIF (Exchangeable image file format) is a standard that specifies the formats to be used for images, sound and tags in digital still cameras and in other systems handling the image and sound files recorded by digital still cameras. Despite the confusing definition and name of the format, EXIF is just a metadata standard. In fact, it simply defines a way to store metadata properties in a variety of well-known image and audio formats. The EXIF tag structure is borrowed from TIFF files. The [specification](https://www.exif.org/Exif2-2.PDF) declares a set of tags intended to store technical details such as the geolocation of the place where a picture was taken, the name of the camera owner, camera settings, etc. 

{{< alert style="info" >}}
Please refer to the [following article](https://en.wikipedia.org/wiki/Exif) to get more information on the standard.
{{< /alert >}}

## Reading basic EXIF properties

To access EXIF metadata in a file of any supported format, GroupDocs.Metadata provides the **IExif.getExifPackage** method. The following are the steps to read EXIF metadata:

1.  [Load]({{< ref "metadata/nodejs-java/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains EXIF metadata
2.  Extract the EXIF metadata package using the **IExif.getExifPackage** method

The following code snippet gets EXIF properties of a TIFF image and displays them on the screen. 

**advanced\_usage.working\_with\_metadata\_standards.exif.ReadBasicExifProperties**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
const metadata = new groupdocs.metadata.Metadata(Constants.TiffWithExif);
    var root = metadata.getRootPackage();
            if (root.getExifPackage() != null) {
                console.log(root.getExifPackage().getArtist());
                console.log(root.getExifPackage().getSoftware());

                // ...

                console.log(root.getExifPackage().getExifIfdPackage().getBodySerialNumber());

                // ...

                console.log(root.getExifPackage().getGpsPackage().getAltitude());

                // ...
            }
```
{{< /tab >}}
{{< /tabs >}}


Here is a full list of tags that can be added to an EXIF package:

*   __TiffAsciiTag__
*   __TiffByteTag__
*   __TiffDoubleTag__
*   __TiffFloatTag__
*   __TiffLongTag__
*   __TiffRationalTag__
*   __TiffSByteTag__
*   __TiffShortTag__
*   __TiffSLongTag__
*   __TiffSRationalTag__
*   __TiffSShortTag__
*   __TiffUndefinedTag__

## Removing EXIF metadata

To remove the EXIF package from a file just pass null to the **IExif.setExifPackage** method. The code sample below shows how to remove EXIF metadata from a file.

**advanced\_usage.working\_with\_metadata\_standards.exif.RemoveExifMetadata**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
const metadata = new groupdocs.metadata.Metadata(Constants.TiffWithExif);
    var root = metadata.getRootPackage();
    root.setExifPackage(null);
    metadata.save(Constants.OutputJpeg);
}
```
{{< /tab >}}
{{< /tabs >}}

## More resources

### Advanced usage topics

To learn more about library features and get familiar how to manage metadata and more, please refer to the[advanced usage section]({{< ref "metadata/nodejs-java/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)
    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)

*   [GroupDocs.Metadata for Node.js via Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Node.js-via-Java)
    

### Free online document metadata management App

Along with a full featured Java library we provide simple, but powerful free Apps.

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
