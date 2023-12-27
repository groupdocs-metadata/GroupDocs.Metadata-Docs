---
id: load-a-file-of-a-specific-format
url: metadata/nodejs-java/load-a-file-of-a-specific-format
title: Load a file of a specific format
weight: 3
description: ""
keywords: 
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
This example demonstrates how to load a file of some particular format.

**advanced\_usage.loading\_files.LoadingFileOfSpecificFormat**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
try {
	var loadOptions = new LoadOptions(FileFormat.Spreadsheet);
    const metadata = new groupdocs.metadata.Metadata("input.xls", loadOptions);
	var root = metadata.getRootPackageGeneric();
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
