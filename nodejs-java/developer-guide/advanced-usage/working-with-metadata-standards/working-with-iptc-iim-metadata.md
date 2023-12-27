---
id: working-with-iptc-iim-metadata
url: metadata/nodejs-java/working-with-iptc-iim-metadata
title: Working with IPTC IIM metadata
weight: 2
description: "This article explains how to access IPTC metadata in a file of any supported format, GroupDocs.Metadata for Node.js via Java provides the IIptc.getIptcPackage method."
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
---
## What is IPTC IIM?

The IPTC Information Interchange Model (IIM) is a set of metadata properties that can be applied to text, images, and other media types. The standard also defines a complex data structure that is utilized to store the properties. The IPTC IIM was developed by the International Press Telecommunications Council (IPTC) to expedite the international exchange of news among newspapers and news agencies. But nowadays it's commonly used by amateur and commercial photographers. The standard is supported by many image creation and manipulation programs. IPTC IIM metadata can be embedded into a variety of image formats such as JPEG, TIFF, etc.

{{< alert style="info" >}}For more information on the IPTC IIM standard please refer to https://www.iptc.org/std/IIM/4.2/specification/IIMV4.2.pdf.{{< /alert >}}

## Reading basic IPTC IIM properties

To access IPTC metadata in a file of any supported format, GroupDocs.Metadata provides the **IIptc.getIptcPackage** method. The following are the steps to read IPTC metadata:

1.  [Load]({{< ref "metadata/nodejs-java/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains IPTC metadata
2.  Extract the IPTC metadata package using the **IIptc.getIptcPackage** method
3.  Read properties of the **IptcApplicationRecord** class instances

The following code snippet gets IPTC properties of a JPEG image and displays them on the screen. 

**advanced\_usage.working\_with\_metadata\_standards.iptc.ReadBasicIptcProperties**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
const metadata = new groupdocs.metadata.Metadata(Constants.JpegWithIptc);
    var root = metadata.getRootPackage();
            if (root.getIptcPackage() != null) {
                if (root.getIptcPackage().getEnvelopeRecord() != null) {
                    console.log(root.getIptcPackage().getEnvelopeRecord().getDateSent());
                    console.log(root.getIptcPackage().getEnvelopeRecord().getDestination());
                    console.log(root.getIptcPackage().getEnvelopeRecord().getFileFormat());
                    console.log(root.getIptcPackage().getEnvelopeRecord().getFileFormatVersion());

                    // ...
                }

                if (root.getIptcPackage().getApplicationRecord() != null) {
                    console.log(root.getIptcPackage().getApplicationRecord().getHeadline());
                    console.log(root.getIptcPackage().getApplicationRecord().getByLine());
                }   
            }
```
{{< /tab >}}
{{< /tabs >}}


## Removing IPTC IIM metadata

To remove the IPTC package from a file just pass null to the **IIptc.setIptcPackage** method as a parameter. The code sample below shows how to remove IPTC metadata from a file.

**advanced\_usage.working\_with\_metadata\_standards.iptc.RemoveIptcMetadata**

{{< tabs "example1">}}
{{< tab "JavaScript" >}}
```js
const metadata = new groupdocs.metadata.Metadata(Constants.JpegWithIptc);
    var root = metadata.getRootPackage();
    root.setIptcPackage(null);
    metadata.save(Constants.OutputJpeg);
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
