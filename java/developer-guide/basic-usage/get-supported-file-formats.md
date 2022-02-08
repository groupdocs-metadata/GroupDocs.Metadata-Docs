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
The [getSupportedFileTypes](https://apireference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/FileType#getSupportedFileTypes()) method of the [FileType](https://apireference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/FileType) class is used to obtain a list of supported file types.

An example of obtaining a list of supported file types is presented below.

```java
Iterable<FileType> supportedFileTypes = FileType.getSupportedFileTypes();
Iterator iterator = supportedFileTypes.iterator();      
while (iterator.hasNext()) {
    FileType fileType = (FileType)iterator.next();
    System.out.println(fileType.getExtension() + " - " + fileType.getDescription());
}
```

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