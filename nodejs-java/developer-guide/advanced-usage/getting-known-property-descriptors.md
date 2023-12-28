---
id: getting-known-property-descriptors
url: metadata/nodejs-java/getting-known-property-descriptors
title: Getting known property descriptors
weight: 7
description: ""
keywords: 
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
This code snippet demonstrates how to extract information about known properties that can be encountered in a particular package.

1.  **Load** a file to examine
2.  Get a collection of **PropertyDescriptor** instances for any desired metadata package
3.  Iterate through the extracted descriptors

**advanced\_usage.GettingKnownPropertyDescriptors**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
const metadata = new groupdocs.metadata.Metadata("input.doc");
    var root = metadata.getRootPackageGeneric();
    var descriptors = root.getDocumentProperties().getKnowPropertyDescriptors();
    for(var i=0;i<descriptors.getCount(); i++)
    {
      var descriptor = descriptors.get_Item(i);
      console.log(descriptor.getName());
      console.log(descriptor.getType().getRawValue());
      console.log(descriptor.getAccessLevel().getRawValue());

      var tags = descriptor.getTags();
      for(var j=0;j<tags.getCount(); j++)
        {
          console.log(tags.get_Item(j).getCategory().toString());
        }
    }
```
{{< /tab >}}
{{< /tabs >}}

{{< alert style="info" >}}
Not all possible properties are presented in the **getKnowPropertyDescriptors** collection. The library provides information on the most frequently used properties only. If there is no descriptor for some property it is still accessible through the GroupDocs.Metadata search engine in read-only mode.
{{< /alert >}}

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
