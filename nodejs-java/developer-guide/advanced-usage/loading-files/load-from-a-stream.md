---
id: load-from-a-stream
url: metadata/nodejs-java/load-from-a-stream
title: Load from a stream
weight: 2
description: ""
keywords: 
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
This example demonstrates how to load a file from a stream.

**advanced\_usage.loading\_files.LoadFromStream**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
try {
	const fileStream = fs.createReadStream("input.one")
    const metadata = new groupdocs.metadata.Metadata(fileStream);
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
