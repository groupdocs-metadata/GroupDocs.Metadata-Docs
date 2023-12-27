---
id: get-document-info
url: metadata/nodejs-java/get-document-info
title: Get document info
weight: 1
description: GroupDocs.Metadata allows users to get meta information of a document.
keywords: meta information
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
GroupDocs.Metadata allows users to get document information which includes:

*   __File format__ (detected by the internal structure)
*   __File extension__
*   __MIME type__
*   __Number of pages__
*   __File size__
*   A __value__ indicating whether a file is encrypted

The following code sample demonstrates how to extract basic format information from a file.

**basic\_usage.GetDocumentInfo**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
const metadata = new groupdocs.metadata.Metadata("input.xlsx");

    if (metadata.getFileFormat() != groupdocs.metadata.FileFormat.Unknown) {
        var info = metadata.getDocumentInfo();
        console.log(`File format: ${info.getFileType().getFileFormat()}`);
        console.log(`File extension: ${info.getFileType().getExtension()}`);
        console.log(`MIME Type: ${info.getFileType().getMimeType()}`);
        console.log(`Number of pages: ${info.getPageCount()}`);
        console.log(`Document size: ${info.getSize()}`);
        console.log(`Is document encrypted: ${info.isEncrypted()}`);
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
