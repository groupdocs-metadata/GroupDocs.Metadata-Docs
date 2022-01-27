---
id: get-supported-file-formats
url: metadata/net/get-supported-file-formats
title: Get supported file formats
weight: 7
description: ""
keywords: 
productName: GroupDocs.Metadata for .NET
hideChildren: False
---
The [GetSupportedFileTypes](https://apireference.groupdocs.com/net/metadata/groupdocs.metadata.common/filetype/methods/getsupportedfiletypes) method of the [FileType](https://apireference.groupdocs.com/net/metadata/groupdocs.metadata.common/filetype) class is used to obtain a list of supported file types.

An example of obtaining a list of supported file types is presented below.

**C#**

```csharp
IEnumerable<FileType> supportedFileTypes = FileType
    .GetSupportedFileTypes()
    .OrderBy(ft => ft.Extension);
 
foreach (FileType fileType in supportedFileTypes)
{
    Console.WriteLine(fileType.Extension.PadRight(8) + " - " + fileType.Description);
}
```

## More resources
### Advanced usage topics
To learn more about document watermarking features and get familiar how to manage watermarks and more, please refer to the [advanced usage section]({{< ref "metadata/net/developer-guide/advanced-usage/_index.md" >}}).

### GitHub examples
You may easily run the code above and see the feature in action in our GitHub examples:
*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)    

### Free online document metadata management App
Along with full featured .NET library we provide simple, but powerful free Apps.
You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).