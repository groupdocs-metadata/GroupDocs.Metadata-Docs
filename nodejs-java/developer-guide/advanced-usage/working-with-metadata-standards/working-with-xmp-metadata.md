---
id: working-with-xmp-metadata
url: metadata/nodejs-java/working-with-xmp-metadata
title: Working with XMP metadata
weight: 3
description: "This article shows how to access XMP metadata in a file of any supported format."
keywords: XMP, XMP metadata
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
## What is XMP?

The Extensible Metadata Platform (XMP) is an XML-based ISO metadata standard, originally created by Adobe Systems Inc. It defines a data structure, serialization model and basic metadata properties intended to form a unified metadata package that can be embedded into different media formats. The defined XMP data model can be used to store any set of metadata properties. These can be simple name/value pairs, structured values or lists of values. The data can be nested as well.

{{< alert style="info" >}}Please find more information on the XMP standard at https://en.wikipedia.org/wiki/Extensible_Metadata_Platform{{< /alert >}}

## Reading XMP properties

To access XMP metadata in a file of any supported format, GroupDocs.Metadata provides the **IXmp.getXmpPackage** method. The following are the steps to read XMP metadata:

1.  **Load** a file that contains XMP metadata
2.  Extract the XMP metadata package using the **IXmp.getXmpPackage** method

The following code snippet gets XMP properties of a PNG image and displays them on the screen. 

**advanced\_usage.working\_with\_metadata\_standards.xmp.ReadXmpProperties**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
const metadata = new groupdocs.metadata.Metadata(Constants.PngWithXmp);
    var root = metadata.getRootPackage();
            if (root.getXmpPackage() != null) {
                if (root.getXmpPackage().getSchemes().getXmpBasic() != null) {
                    console.log(root.getXmpPackage().getSchemes().getXmpBasic().getCreatorTool());
                    console.log(root.getXmpPackage().getSchemes().getXmpBasic().getCreateDate());
                }

                if (root.getXmpPackage().getSchemes().getDublinCore() != null) {
                    console.log(root.getXmpPackage().getSchemes().getDublinCore().getFormat());
                    console.log(root.getXmpPackage().getSchemes().getDublinCore().getCoverage());
                }

                if (root.getXmpPackage().getSchemes().getPhotoshop() != null) {
                    console.log(root.getXmpPackage().getSchemes().getPhotoshop().getColorMode());
                    console.log(root.getXmpPackage().getSchemes().getPhotoshop().getIccProfile());
                }
            }
```
{{< /tab >}}
{{< /tabs >}}

Here is a full list of supported XMP schemes:

*   __mpBasicJobTicketPackage__
*   __mpBasicPackage__
*   __mpCameraRawPackage__
*   __mpDublinCorePackage__
*   __mpDynamicMediaPackage__
*   __mpIptcCorePackage__
*   __mpIptcExtensionPackage__
*   __mpIptcIimPackage__
*   __mpMediaManagementPackage__
*   __mpPagedTextPackage__
*   __mpPdfPackage__
*   __mpPhotoshopPackage__
*   __mpRightsManagementPackage__

{{< alert style="info" >}}GroupDocs.Metadata also provides an API allowing users to work with fully custom XMP schemes/packages. Please refer to this code snippet to learn more.{{< /alert >}}

## Removing XMP metadata

To remove the XMP package from a file just pass null to the **IXmp.setXmpPackage** method as a parameter. The code sample below shows how to remove XMP metadata from a file.

**advanced\_usage.working\_with\_metadata\_standards.xmp.AddCustomXmpPackage**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
 const metadata = new groupdocs.metadata.Metadata(Constants.JpegWithXmp);
    var root = metadata.getRootPackage();
    root.setXmpPackage(null);
    metadata.save(Constants.OutputJpeg);
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
