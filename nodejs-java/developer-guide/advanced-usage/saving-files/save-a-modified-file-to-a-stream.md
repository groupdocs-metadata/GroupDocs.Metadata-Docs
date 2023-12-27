---
id: save-a-modified-file-to-a-stream
url: metadata/nodejs-java/save-a-modified-file-to-a-stream
title: Save a modified file to a stream
weight: 3
description: "This article shows how to save a file to the specified stream in Java"
keywords: save a file to the specified stream
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
This example shows how to save a file to the specified stream.

**AdvancedUsage.SavingFiles.SaveFileToSpecifiedStream**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
const fileStream = fs.createReadStream("output.pdf")
const metadata = new groupdocs.metadata.Metadata("input.pdf");

    // Edit or remove metadata here

metadata.save(fileStream);
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
