---
id: working-with-ogg-metadata
url: metadata/java/working-with-ogg-metadata
title: Working with OGG metadata
weight: 3
description: "This article shows how to read Vorbis comments of OGG audio files with GroupDocs.Metadata for Java."
keywords: Java OGG Vorbis comments metadata artist title album GroupDocs.Metadata
productName: GroupDocs.Metadata for Java
hideChildren: False
---
OGG Vorbis files store their metadata as Vorbis comments: pairs of a field name and a value, such as `ARTIST=...` or `TITLE=...`. GroupDocs.Metadata for Java reads these comments through the [OggPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/OggPackage) class.

The most common fields have dedicated getters:

| Group | Getters |
|-------|---------|
| Track | `getTitle`, `getAlbum`, `getTracknumber`, `getArtist`, `getPerformer`, `getGenre`, `getDate` |
| Rights and contacts | `getCopyright`, `getLicense`, `getOrganization`, `getDescription`, `getLocation`, `getContact`, `getIsrc` |
| Encoder | `getVendor`, `getVersion` |

All other comments are available through [getOggUserComments](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/OggPackage#getOggUserComments()).

## Reading OGG Vorbis comments

1.  [Load]({{< ref "metadata/java/developer-guide/advanced-usage/loading-files/_index.md" >}}) an OGG file
2.  Get the root metadata package
3.  Extract the native metadata package using the [OggRootPackage.getOggPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/OggRootPackage#getOggPackage()) method
4.  Read the properties of the package

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>audio.ogg.OggReadNativeMetadataProperties**

```java
try (Metadata metadata = new Metadata(Constants.InputOgg)) {
    OggRootPackage root = metadata.getRootPackageGeneric();
    OggPackage ogg = root.getOggPackage();

    System.out.println(ogg.getVendor());
    System.out.println(ogg.getTitle());
    System.out.println(ogg.getArtist());
    System.out.println(ogg.getAlbum());
    System.out.println(ogg.getGenre());
    System.out.println(ogg.getDate());
    System.out.println(ogg.getCopyright());

    // Vorbis comments that are not mapped to dedicated properties
    for (OggUserComment comment : ogg.getOggUserComments()) {
        System.out.println(String.format("%s = %s", comment.getHeader(), comment.getValue()));
    }
}
```

## Searching OGG properties by tags

The OGG properties are marked with the same [tags]({{< ref "metadata/java/developer-guide/basic-usage/find-metadata-properties.md" >}}) as the properties of other audio formats. So you can find, for example, the artist and the title without knowing the format of the file.

```java
try (Metadata metadata = new Metadata(Constants.InputOgg)) {
    IReadOnlyList<MetadataProperty> properties = metadata.findProperties(
            new ContainsTagSpecification(Tags.getPerson().getArtist())
                    .or(new ContainsTagSpecification(Tags.getContent().getTitle())));

    for (MetadataProperty property : properties) {
        System.out.println(String.format("%s = %s", property.getName(), property.getValue()));
    }
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)

*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)

### Free online document metadata management App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
