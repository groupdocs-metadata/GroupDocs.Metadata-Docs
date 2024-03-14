---
id: load-from-a-local-disk
url: metadata/net/load-from-a-local-disk
title: Load from a local disk
weight: 1
description: "The following example demonstrates how to load file from local disk."
keywords: load file from local disk
productName: GroupDocs.Metadata for .NET
hideChildren: False
---
The following example demonstrates how to load a file from a local disk.

**AdvancedUsage.LoadingFiles.LoadFromLocalDisk**

```csharp
var inputPath = @"C:\Docs\source.one";
using (Metadata metadata = new Metadata(inputPath))
{
	// Extract, edit or remove metadata here
}
```

## More resources
### GitHub examples
You may easily run the code above and see the feature in action in our GitHub examples:
*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)    

### Free online document metadata management App
Along with full featured .NET library we provide simple, but powerful free Apps.
You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
