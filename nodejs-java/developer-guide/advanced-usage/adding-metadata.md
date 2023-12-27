---
id: adding-metadata
url: metadata/nodejs-java/adding-metadata
title: Adding metadata
weight: 5
description: "This article shows how to add metadata properties which is the most sophisticated feature of the GroupDocs.Metadata Node.js via Java search engine"
keywords: Adding metadata, Adding metadata properties
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
Adding metadata properties is the most sophisticated feature of the GroupDocs.Metadata search engine. When you call the **addProperties** method it examines all available metadata packages and tries to pick up a known property that would satisfy the specified predicate. Note that the property will be added to metadata packages that fit the following criteria: 

1.  Only existing metadata packages will be affected. No new packages are added during this operation
2.  There should be a known metadata property in the package structure that fits the search condition but is actually missing in the package. All properties supported by a certain package are usually defined in the specification of a particular metadata standard

**advanced\_usage.AddingMetadata**

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
                var affected = metadata.addProperties(new groupdocs.metadata.ContainsTagSpecification(groupdocs.metadata.Tags.getTime().getPrinted()), new groupdocs.metadata.PropertyValue(new Date()));

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
