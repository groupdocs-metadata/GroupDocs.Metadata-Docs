---
id: load-from-a-uri
url: metadata/java/load-from-a-uri
title: Load from a URI
weight: 5
description: "This article shows how to load a document from a web resource by its URI with GroupDocs.Metadata for Java."
keywords: Java load metadata from URL URI web resource GroupDocs.Metadata
productName: GroupDocs.Metadata for Java
hideChildren: False
---
GroupDocs.Metadata for Java can load a document straight from a web resource. Pass a [java.net.URI](https://docs.oracle.com/javase/8/docs/api/java/net/URI.html) to the [Metadata](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata/Metadata) constructor, and you don't have to download the file yourself.

The document is downloaded into memory and then loaded the same way as a document loaded [from a stream]({{< ref "metadata/java/developer-guide/advanced-usage/loading-files/load-from-a-stream.md" >}}).

## Loading a document from a URI

The following code sample shows how to load an image from a URL and read its EXIF properties.

**advanced\_usage.loading\_files.LoadFromUri**

```java
// The document is downloaded into memory and then loaded like a stream
URI uri = URI.create("https://raw.githubusercontent.com/groupdocs-metadata/GroupDocs.Metadata-for-Java/master/Examples/Resources/SampleFiles/Input/exif.jpg");

try (Metadata metadata = new Metadata(uri)) {
    JpegRootPackage root = metadata.getRootPackageGeneric();
    System.out.println(root.getFileType().getFileFormat());

    if (root.getExifPackage() != null) {
        System.out.println(root.getExifPackage().getMake());
        System.out.println(root.getExifPackage().getModel());
    }
}
```

## Loading a document of a specific format from a URI

Use the `Metadata(URI, LoadOptions)` constructor to set the file format explicitly or to pass a password.

RAR, 7z, TAR, AAR, GZIP, BZ2, XZ and Zstandard archives are recognized by the file extension, and a URI does not give the library a file name. Set the format of such an archive explicitly, otherwise the archive is not recognized correctly (a RAR archive, for example, is detected as an MP3 file).

```java
URI uri = URI.create("https://raw.githubusercontent.com/groupdocs-metadata/GroupDocs.Metadata-for-Java/master/Examples/Resources/SampleFiles/Input/input.rar");

try (Metadata metadata = new Metadata(uri, new LoadOptions(FileFormat.Rar))) {
    RarRootPackage root = metadata.getRootPackageGeneric();
    System.out.println(root.getRarPackage().getTotalEntries());

    for (RarFile file : root.getRarPackage().getFiles()) {
        System.out.println(file.getName());
    }
}
```

{{< alert style="info" >}}
The whole document is downloaded into memory before it is processed. For large files, consider downloading them to a local disk first and [loading them from the path]({{< ref "metadata/java/developer-guide/advanced-usage/loading-files/load-from-a-local-disk.md" >}}).
{{< /alert >}}

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)

*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)

### Free online document metadata management App

Along with a full featured Java library we provide simple, but powerful free Apps.

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
