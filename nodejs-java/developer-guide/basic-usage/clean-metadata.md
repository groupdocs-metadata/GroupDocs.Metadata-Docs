---
id: clean-metadata
url: metadata/nodejs-java/clean-metadata
title: Clean metadata
weight: 5
description: Sometimes you may need to just remove all or clean metadata properties without applying any filters. The best way to do this is to use the Sanitize method.
keywords: clean metadata, metadata
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
## Remove all recognized metadata properties from a file

Sometimes you may need to just remove all or clean metadata properties without applying any filters. The best way to do this is to use the **sanitize** method.

This example demonstrates how to remove all detected metadata packages/properties.

1.  **Load** a file to clean
2.  Call the **sanitize** method
3.  Check the actual number of removed packages/properties
4.  **Save** the changes

**basic\_usage.CleanMetadata**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
const metadata = new groupdocs.metadata.Metadata("input.pdf");

    // Remove detected metadata packages
    var affected = metadata.sanitize();
    console.log(`\nProperties removed: ${affected}.`);

    metadata.save("output.pdf");
}
```
{{< /tab >}}
{{< /tabs >}}

As a result, we get a sanitized version of the original file.

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
