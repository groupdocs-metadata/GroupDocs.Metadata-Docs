---
id: working-with-metadata-in-avif-images
url: metadata/java/working-with-metadata-in-avif-images
title: Working with metadata in AVIF images
weight: 11
description: "This article shows how to read EXIF and XMP metadata of AVIF images with GroupDocs.Metadata for Java."
keywords: Java AVIF image metadata EXIF XMP GroupDocs.Metadata
productName: GroupDocs.Metadata for Java
hideChildren: False
---
AVIF (AV1 Image File Format) stores images compressed with the AV1 codec in a HEIF container. Like other modern image formats, AVIF files can carry EXIF and XMP metadata. The [AvifRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/AvifRootPackage) class implements both the [IExif](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/IExif) and [IXmp](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/IXmp) interfaces.

## Reading EXIF and XMP properties

1.  [Load]({{< ref "metadata/java/developer-guide/advanced-usage/loading-files/_index.md" >}}) an AVIF image
2.  Get the root metadata package
3.  Read the XMP package with [getXmpPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/AvifRootPackage#getXmpPackage()) and the EXIF package with [getExifPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/AvifRootPackage#getExifPackage()). Both can be `null` if the image has no such metadata.

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>image.avif.AvifReadProperties**

```java
try (Metadata metadata = new Metadata(Constants.InputAvif)) {
    AvifRootPackage root = metadata.getRootPackageGeneric();

    if (root.getXmpPackage() != null && root.getXmpPackage().getSchemes().getXmpDynamicMedia() != null) {
        System.out.println(String.format("XmpDynamicMedia Artist: %s", root.getXmpPackage().getSchemes().getXmpDynamicMedia().getArtist()));
    }

    if (root.getExifPackage() != null) {
        System.out.println(String.format("ExifPackage Artist: %s", root.getExifPackage().getArtist()));
    }
}
```

## Working with XMP metadata

For more details on reading and updating XMP metadata, please refer to the following guide: [Working with XMP metadata]({{< ref "metadata/java/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata.md" >}}).

## Working with EXIF metadata

Please find more code samples in the [Working with EXIF metadata]({{< ref "metadata/java/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata.md" >}}) section.

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)

*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)

### Free online document metadata management App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
