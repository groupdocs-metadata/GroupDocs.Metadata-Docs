---
id: working-with-metadata-in-tiff-images
url: metadata/java/working-with-metadata-in-tiff-images
title: Working with metadata in TIFF images
weight: 9
description: ""
keywords: 
productName: GroupDocs.Metadata for Java
hideChildren: False
---
## Working with XMP metadata

GroupDocs.Metadata for Java allows managing XMP metadata in TIFF images. For more details please refer to the following guide: [Working with XMP Metadata]({{< ref "metadata/java/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata.md" >}}).

## Working with EXIF metadata

The GroupDocs.Metadata API supports handling EXIF metadata in TIFF images. Please find appropriate code samples in the [Working with EXIF Metadata]({{< ref "metadata/java/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata.md" >}}) section.

The code sample below shows how to update EXIF properties in a TIFF image.

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>image.tiff.TiffUpdateExifProperties**

```java
try (Metadata metadata = new Metadata(Constants.TiffWithExif)) {
    IExif root = (IExif) metadata.getRootPackage();

    // Set the EXIF package if it's missing
    if (root.getExifPackage() == null) {
        root.setExifPackage(new ExifPackage());
    }

    root.getExifPackage().setCopyright("Copyright (C) 2011-2026 GroupDocs. All Rights Reserved.");
    root.getExifPackage().setImageDescription("test TIFF image");
    root.getExifPackage().setSoftware("GroupDocs.Metadata");
    root.getExifPackage().setArtist("GroupDocs");
    // ...

    root.getExifPackage().getExifIfdPackage().setBodySerialNumber("test");
    root.getExifPackage().getExifIfdPackage().setCameraOwnerName("GroupDocs");
    root.getExifPackage().getExifIfdPackage().setUserComment("test comment");
    // ...

    metadata.save(Constants.OutputTiff);
}
```

## Working with IPTC metadata

GroupDocs.Metadata for Java is also able to work with IPTC metadata in TIFF images. Please find more information in the [Working with IPTC IIM Metadata]({{< ref "metadata/java/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata.md" >}}) section.

The code sample below shows how to update IPTC properties in a TIFF image. If the image has no IPTC package or the required records yet, they are created first.

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>image.tiff.TiffUpdateIptcProperties**

```java
try (Metadata metadata = new Metadata(Constants.TiffWithIptc)) {
    IIptc root = (IIptc) metadata.getRootPackage();

    // Set the IPTC package if it's missing
    if (root.getIptcPackage() == null) {
        root.setIptcPackage(new IptcRecordSet());
    }

    if (root.getIptcPackage().getEnvelopeRecord() == null) {
        root.getIptcPackage().setEnvelopeRecord(new IptcEnvelopeRecord());
    }
    root.getIptcPackage().getEnvelopeRecord().setDateSent(new Date());
    root.getIptcPackage().getEnvelopeRecord().setProductID(UUID.randomUUID().toString());
    // ...

    if (root.getIptcPackage().getApplicationRecord() == null) {
        root.getIptcPackage().setApplicationRecord(new IptcApplicationRecord());
    }
    root.getIptcPackage().getApplicationRecord().setByLine("GroupDocs");
    root.getIptcPackage().getApplicationRecord().setHeadline("test headline");
    root.getIptcPackage().getApplicationRecord().setByLineTitle("code sample");
    root.getIptcPackage().getApplicationRecord().setReleaseDate(new Date());
    root.getIptcPackage().getApplicationRecord().setCity("New York");
    root.getIptcPackage().getApplicationRecord().setCaptionAbstract("test caption");
    // ...

    metadata.save(Constants.OutputTiff);
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)
    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)
    

### TIFF metadata management demo App
The [TIFF Metadata Editing](https://products.groupdocs.app/metadata/tiff) app is available to view and edit metadata of TIFF images, including EXIF, XMP, IPTC, and other properties.
