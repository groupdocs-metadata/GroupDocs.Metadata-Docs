---
id: evaluation-limitations-and-licensing
url: metadata/nodejs-java/evaluation-limitations-and-licensing
title: Evaluation Limitations and Licensing
weight: 5
description: "GroupDocs.Metadata for Node.js provides different plans for purchase or offers a Free Trial and a 30-day Temporary License for evaluation."
keywords: free, free trial, evaluation, groupdocs metadata java
productName: GroupDocs.Metadata for Node.js via Java
hideChildren: False
toc: True
---

To study the system, you may want quick access to the API. To make this easier, GroupDocs.Metadata provides different plans for purchase and offers a Free Trial and a 30-day Temporary License for evaluation.

{{< alert style="info" >}}GroupDocs.Metadata also works without the license in trial mode with certain limitations{{< /alert >}}

## Purchased License

After buying, apply the license file or include it as an embedded resource. 

License needs to be set:
- Only once per application domain
- Before using any other GroupDocs.Viewer classes

## Evaluation version limitations

You can easily download GroupDocs.Metadata for evaluation. The evaluation download is the same as the purchased download. The evaluation version simply becomes licensed when you add a few lines of code to apply the license. You will face following limitations while using the API without the license.  

| API | Limitations |
| --- | --- |
| Document properties (Pdf, Word, Excel, PowerPoint, Visio, etc) | Only first 5 properties can be read |
| XMP API | Only first 2 XMP schemes can be read |
| EXIF API | GPS IFD and image thumbnail are unavailable  
Only first 5 properties can be read |
| IPTC API | Only first 5 properties can be read |
| Id3v2, Lyrics3, APEv2 tags | Only first 5 properties can be read |
| QuickTime atoms | Only first 5 atoms can be read |
| File open | Open maximum 15 files, otherwise, API throws exception |
| File save | Not supported in trial mode |

## Licensing 

The license file contains details such as the product name, number of developers it is licensed to, subscription expiry date and so on. It contains the digital signature, so don't modify the file. Even inadvertent addition of an extra line break into the file will invalidate it. You need to set a license before utilizing GroupDocs.Metadata API if you want to avoid its evaluation limitations. 

The license can be loaded from a file or stream object.

### Applying license from file

The code below will explain how to apply a  product license.

{{< tabs "example1">}}
{{< tab "JavaScript" >}}

```js
const licensePath = "path to the .lic file";
const license = new groupdocs.metadata.License()
license.setLicense(licensePath); 
```

{{< /tab >}}
{{< /tabs >}}

### Applying license from stream

The following code snippet shows how to set a license from a stream:

{{< tabs "example2">}}
{{< tab "JavaScript" >}}

```js
const fs = require('fs')  
  
const licensePath = "path to the .lic file"
const license = new groupdocs.metadata.License()
const licStream = fs.createReadStream(licensePath)
groupdocs.metadata.License.setLicenseFromStream(license, licStream, err => {
  if (err) {
    console.log(`Set license error: ${err}`)
  } else {
    console.log('License set OK')
  }
})
```

{{< /tab >}}
{{< /tabs >}}


{{< alert style="info" >}}
Calling [License.setLicense](https://reference.groupdocs.com/metadata/nodejs-java/com.groupdocs.metadata.licensing/License#setLicense(java.lang.String)) multiple times is not harmful but simply wastes processor time. It is called once when the application starts.
{{< /alert >}}
