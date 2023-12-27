---
id: updating-metadata
url: metadata/nodejs-java/updating-metadata
title: Updating metadata
weight: 4
description: "The Example in this article demonstrates that how to update metadata properties using a combination of criteria"
keywords: metadata properties, update existing metadata, update metadata properties
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
The example below demonstrates how to update existing metadata properties using a combination of criteria. Please note that the **updateProperties** method checks the type of all properties before applying any changes. If a property satisfies the predicate but has a type different from the passed value it won't be updated. The explicit type check in the example is performed since we use the existing value to filter metadata properties.

1.  [Open]({{< ref "metadata/nodejs-java/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file to be updated
2.  Specify a predicate that will be used to filter desired metadata properties
3.  Specify a value which you want to be assigned to the selected properties
4.  Pass the predicate and the new value to the **updateProperties** method
5.  Check the actual number of updated properties
6.  [Save]({{< ref "metadata/nodejs-java/developer-guide/advanced-usage/saving-files/_index.md" >}}) the changes

**advanced\_usage.UpdatingMetadata**

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
                var affected = metadata.updateProperties(new groupdocs.metadata.ContainsTagSpecification(groupdocs.metadata.Tags.getTime().getPrinted()), new groupdocs.metadata.PropertyValue(new Date()));

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
