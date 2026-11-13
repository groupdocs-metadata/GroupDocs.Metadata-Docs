---
id: get-supported-file-formats
url: metadata/java/get-supported-file-formats
title: Get supported file formats
weight: 7
description: ""
keywords: 
productName: GroupDocs.Metadata for Java
hideChildren: False
---
The [getSupportedFileTypes](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/FileType#getSupportedFileTypes()) method of the [FileType](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/FileType) class is used to obtain a list of supported file types.

An example of obtaining a list of supported file types is presented below.

```java
Iterable<FileType> supportedFileTypes = FileType.getSupportedFileTypes();
Iterator iterator = supportedFileTypes.iterator();      
while (iterator.hasNext()) {
    FileType fileType = (FileType)iterator.next();
    System.out.println(fileType.getExtension() + " - " + fileType.getDescription());
}
```

## Get supported features for a file type

Not every format supports every operation. Some formats can only be read, for example. The [FileTypeFeatureSupport](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/FileTypeFeatureSupport) class describes which features are available for a file type:

| Feature ([ProductFeature](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/ProductFeature)) | Meaning |
|---------|---------|
| `AutoDetection` | The format is detected by the file content |
| `Read` | Metadata can be read |
| `Edit` | Metadata can be added or updated |
| `Remove` | Metadata can be removed |

Each feature has a [SupportLevel](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/SupportLevel): `Supported`, `PartiallySupported` or `NotSupported`.

The [getSupportedFileTypeFeatures](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/FileTypeFeatureSupport#getSupportedFileTypeFeatures()) method has three overloads: it accepts a [FileType](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/FileType), a file extension, or no arguments to return the whole matrix.

**advanced\_usage.GetSupportedFileTypeFeatures**

```java
public class GetSupportedFileTypeFeatures {
    public static void run() {
        // Get the support matrix for a specific file type
        FileTypeFeatureSupport pdf = FileTypeFeatureSupport.getSupportedFileTypeFeatures(FileType.PDF);
        printFeatures(pdf);

        // ... or look it up by file extension
        FileTypeFeatureSupport ogg = FileTypeFeatureSupport.getSupportedFileTypeFeatures(".ogg");
        printFeatures(ogg);

        // Get the whole matrix
        Map<FileType, FileTypeFeatureSupport> matrix = FileTypeFeatureSupport.getSupportedFileTypeFeatures();
        System.out.println(String.format("Total file types described: %d", matrix.size()));
    }

    private static void printFeatures(FileTypeFeatureSupport support) {
        System.out.println(String.format("%s (%s): %s", support.getExtension(), support.getFormatFamily(), support.getDescription()));
        for (FeatureSupport feature : support.getFeatures()) {
            System.out.println(String.format("    %s: %s", feature.getFeature(), feature.getLevel()));
        }
    }
}
```

The following helper checks whether a file can be edited before you try to update it:

```java
static boolean canEdit(String extension) {
    FileTypeFeatureSupport support = FileTypeFeatureSupport.getSupportedFileTypeFeatures(extension);
    for (FeatureSupport feature : support.getFeatures()) {
        if (feature.getFeature() == ProductFeature.Edit) {
            return feature.getLevel() != SupportLevel.NotSupported;
        }
    }
    return false;
}
```

For example, `canEdit(".docx")` returns `true` and `canEdit(".gz")` returns `false`.

{{< alert style="info" >}}
For an extension that is not registered, `getSupportedFileTypeFeatures(String)` throws `java.util.NoSuchElementException` instead of returning `null`. Catch the exception if the extension comes from user input.
{{< /alert >}}

## More resources

### Advanced usage topics

To learn more about library features and get familiar how to manage metadata and more, please refer to the[advanced usage section]({{< ref "metadata/java/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)
    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)
    

### Free online document metadata management App

Along with a full featured Java library we provide simple, but powerful free Apps.

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).