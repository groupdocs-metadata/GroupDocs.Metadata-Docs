---
id: working-with-interpreted-values
url: metadata/nodejs-java/working-with-interpreted-values
title: Working with interpreted values
weight: 8
description: "This article contains a good example of numeric flag or enumeration in Node.js via Java."
keywords: particular metadata property
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
Sometimes it's not really obvious what a particular metadata property is supposed to mean. A good example of such vague property is a numeric flag or enumeration. 
From release to release, we add interpreters to metadata properties extracted from various formats. To get a full list of properties having interpreted values for a particular file please use the below example:

**advanced_usage.WorkingWithInterpretedValues**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
const metadata = new groupdocs.metadata.Metadata(Constants.InputDoc);
    var properties = metadata.findProperties(new groupdocs.metadata.OfTypeSpecification(groupdocs.metadata.MetadataPropertyType.Integer));
      for(var i=0;i<properties.getCount(); i++){
        var property = properties.get_Item(i);
        if(property.getInterpretedValue() != null)
        {
          console.log(property.getName());
          console.log(property.getValue().getRawValue());
          console.log(property.getInterpretedValue().getRawValue());
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
