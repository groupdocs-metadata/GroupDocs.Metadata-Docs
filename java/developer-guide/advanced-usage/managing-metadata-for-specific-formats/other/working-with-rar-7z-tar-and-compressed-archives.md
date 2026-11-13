---
id: working-with-rar-7z-tar-and-compressed-archives
url: metadata/java/working-with-rar-7z-tar-and-compressed-archives
title: Working with RAR, 7z, TAR and compressed archives
weight: 8
description: "This article shows how to read metadata of RAR, 7z, TAR, Apple Archive (AAR), GZIP, BZ2, XZ and Zstandard archives with GroupDocs.Metadata for Java."
keywords: Java archive metadata RAR 7z TAR AAR GZIP BZ2 XZ Zstandard ZST GroupDocs.Metadata
productName: GroupDocs.Metadata for Java
hideChildren: False
---
Besides [ZIP archives]({{< ref "metadata/java/developer-guide/advanced-usage/managing-metadata-for-specific-formats/other/working-with-zip-archives.md" >}}), GroupDocs.Metadata for Java reads format metadata of the following archive types:

| Format | Extensions | Root package | What you can read |
|--------|-----------|--------------|-------------------|
| RAR | `.rar`, `.cbr` | [RarRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/RarRootPackage) | Total entries; name, sizes and modification date of each file |
| 7z | `.7z`, `.cb7` | [SevenZipRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/SevenZipRootPackage) | Total entries; name, sizes and modification date of each file |
| TAR | `.tar`, `.cbt` | [TarRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/TarRootPackage) | Total entries; name and size of each file |
| Apple Archive | `.aar` | [AarRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/AarRootPackage) | Total entries, solid flag; name, length and directory flag of each entry |
| GZIP | `.gz`, `.gzip` | [GzipRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/GzipRootPackage) | Name, total entries, compressed and uncompressed size |
| BZ2 | `.bz2` | [Bz2RootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/Bz2RootPackage) | Name, total entries, size |
| XZ | `.xz` | [XzRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/XzRootPackage) | Name, total entries, size |
| Zstandard | `.zst`, `.zstd` | [ZstdRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/ZstdRootPackage) | Name, total entries, size |

The properties of these packages are read-only. To find out what is supported for a particular format, see [Get supported file formats]({{< ref "metadata/java/developer-guide/basic-usage/get-supported-file-formats.md" >}}).

## Reading RAR archive metadata

1.  [Load]({{< ref "metadata/java/developer-guide/advanced-usage/loading-files/_index.md" >}}) a RAR archive
2.  Get the root metadata package
3.  Read the total number of entries with [RarPackage.getTotalEntries](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/RarPackage#getTotalEntries())
4.  Loop through [RarPackage.getFiles](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/RarPackage#getFiles()) to extract information about the archived files

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>archive.RarReadNativeMetadataProperties**

```java
try (Metadata metadata = new Metadata(Constants.InputRar)) {
    RarRootPackage root = metadata.getRootPackageGeneric();
    System.out.println(root.getRarPackage().getTotalEntries());

    for (RarFile file : root.getRarPackage().getFiles()) {
        System.out.println(file.getName());
        System.out.println(file.getCompressedSize());
        System.out.println(file.getModificationDateTime());
        System.out.println(file.getUncompressedSize());
    }
}
```

## Reading 7z archive metadata

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>archive.SevenZipReadNativeMetadataProperties**

```java
try (Metadata metadata = new Metadata(Constants.InputSevenZip)) {
    SevenZipRootPackage root = metadata.getRootPackageGeneric();
    System.out.println(root.getSevenZipPackage().getTotalEntries());

    for (SevenZipFile file : root.getSevenZipPackage().getFiles()) {
        System.out.println(file.getName());
        System.out.println(file.getCompressedSize());
        System.out.println(file.getModificationDateTime());
        System.out.println(file.getUncompressedSize());
    }
}
```

## Reading TAR archive metadata

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>archive.TarReadNativeMetadataProperties**

```java
try (Metadata metadata = new Metadata(Constants.InputTar)) {
    TarRootPackage root = metadata.getRootPackageGeneric();
    System.out.println(root.getTarPackage().getTotalEntries());

    for (TarFile file : root.getTarPackage().getFiles()) {
        System.out.println(file.getName());
        System.out.println(file.getSize());
    }
}
```

## Reading Apple Archive (AAR) metadata

An Apple Archive can be solid, that is compressed as a single stream. The [AarPackage.isSolid](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/AarPackage#isSolid()) method tells you which kind of archive you have.

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>archive.AarReadNativeMetadataProperties**

```java
try (Metadata metadata = new Metadata(Constants.InputAar)) {
    AarRootPackage root = metadata.getRootPackageGeneric();
    AarPackage archive = root.getAarPackage();

    System.out.println(archive.getTotalEntries());
    System.out.println(archive.isSolid());

    for (AarFile file : archive.getFiles()) {
        System.out.println(file.getName());
        System.out.println(file.getLength());
        System.out.println(file.isDirectory());
    }
}
```

## Reading GZIP archive metadata

A GZIP file compresses a single file. Its package contains the name of the compressed file and both the compressed and the uncompressed size.

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>archive.GzipReadNativeMetadataProperties**

```java
try (Metadata metadata = new Metadata(Constants.InputGzip)) {
    GzipRootPackage root = metadata.getRootPackageGeneric();
    GzipPackage archive = root.getGzipPackage();

    System.out.println(archive.getTotalEntries());
    System.out.println(archive.getName());
    System.out.println(archive.getUncompressedSize());
    System.out.println(archive.getSize());
}
```

## Reading BZ2, XZ and Zstandard archive metadata

BZ2, XZ and Zstandard packages have the same set of properties: the name of the compressed file, the total number of entries and the size. The code below reads a BZ2 archive.

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>archive.Bz2ReadNativeMetadataProperties**

```java
try (Metadata metadata = new Metadata(Constants.InputBz2)) {
    Bz2RootPackage root = metadata.getRootPackageGeneric();
    Bz2Package archive = root.getBz2Package();

    System.out.println(archive.getTotalEntries());
    System.out.println(archive.getName());
    System.out.println(archive.getSize());
}
```

For the other two formats, use the matching root package:

| Format | Root package | Getter | Example |
|--------|--------------|--------|---------|
| XZ | `XzRootPackage` | `getXzPackage()` | `archive.XzReadNativeMetadataProperties` |
| Zstandard | `ZstdRootPackage` | `getZstdPackage()` | `archive.ZstdReadNativeMetadataProperties` |

```java
try (Metadata metadata = new Metadata(Constants.InputZstd)) {
    ZstdRootPackage root = metadata.getRootPackageGeneric();
    ZstdPackage archive = root.getZstdPackage();

    System.out.println(archive.getTotalEntries());
    System.out.println(archive.getName());
    System.out.println(archive.getSize());
}
```

## Loading archives from a stream

The formats on this page are recognized by the file extension. When an archive is loaded from an `InputStream` or a [URI]({{< ref "metadata/java/developer-guide/advanced-usage/loading-files/load-from-a-uri.md" >}}), there is no file name, so set the format explicitly with [LoadOptions]({{< ref "metadata/java/developer-guide/advanced-usage/loading-files/load-a-file-of-a-specific-format.md" >}}). Without it, the archive is not recognized correctly: a RAR archive, for example, is detected as an MP3 file.

```java
try (InputStream stream = new FileInputStream(Constants.InputRar);
     Metadata metadata = new Metadata(stream, new LoadOptions(FileFormat.Rar))) {
    RarRootPackage root = metadata.getRootPackageGeneric();
    System.out.println(root.getRarPackage().getTotalEntries());
}
```

The matching [FileFormat](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/FileFormat) values are `Rar`, `SevenZip`, `Tar`, `Aar`, `Gzip`, `Bz2`, `Xz` and `Zstd`.

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)

*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)

### Free online document metadata management App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
