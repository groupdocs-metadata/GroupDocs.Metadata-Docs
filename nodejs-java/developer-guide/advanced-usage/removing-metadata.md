---
id: removing-metadata
url: metadata/nodejs-java/removing-metadata
title: Removing metadata
weight: 3
description: ""
keywords: 
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
Not all metadata properties extracted from a file are marked with tags. Some file formats and metadata standards allow adding fully custom properties that can't be properly tagged by the library since their purpose is not clearly defined in the appropriate format/standard specification. In such cases, you can use the name of the property to locate and remove it. The following example demonstrates some advanced usage scenarios of the GroupDocs.Metadata search engine allowing to remove metadata properties.

1.  [Load]({{< ref "metadata/nodejs-java/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file to be modified
2.  Pass a search predicate to the **removeProperties** method.
3.  Check the number of properties that were actually removed
4.  [Save]({{< ref "metadata/nodejs-java/developer-guide/advanced-usage/saving-files/_index.md" >}}) the changes

**advanced\_usage.RemovingMetadata**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
fs.readdirSync(Constants.inputPath).forEach(file => {
            var metadata = new groupdocs.metadata.Metadata(Constants.inputPath + file);
            if (metadata.getFileFormat() != groupdocs.metadata.FileFormat.Unknown && !metadata.getDocumentInfo().isEncrypted()) {
                
                console.log(file);

                // Add a property containing the file last printing date if it's missing
                // Note that the property will be added to metadata packages that satisfy the following criteria:
                // 1) Only existing metadata packages will be affected. No new packages are added during this operation
                // 2) There should be a known metadata property in the package structure that fits the search condition but is actually missing in the package.
                // All properties supported by a certain package are usually defined in the specification of a particular metadata standard
                var affected = metadata.removeProperties(new groupdocs.metadata.ContainsTagSpecification(groupdocs.metadata.Tags.getTime().getPrinted()));

                console.log(`Affected properties: ${affected}`);
            }
      });
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
