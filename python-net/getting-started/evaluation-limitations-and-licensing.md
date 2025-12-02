---
id: evaluation-limitations-and-licensing
url: metadata/python-net/evaluation-limitations-and-licensing
title: Evaluation Limitations and Licensing
weight: 5
description: "GroupDocs.Metadata for Python provides different plans for purchase or offers a Free Trial and a 30-day Temporary License for evaluation."
keywords: free, free trial, evaluation, groupdocs metadata java
productName: GroupDocs.Metadata for Python via .NET
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



```python
import groupdocs.metadata as gm
import constants
import os
  

def run():
    if os.path.exists(constants.license_path):    
        license = gm.License()
        license.set_license(constants.license_path)
        print("License set successfully.")
    else:
       print("\n")
```



### Applying license from stream

The following code snippet shows how to set a license from a stream:



```python
import groupdocs.metadata as gm
import constants
import os
from os.path import join

def run():
    if os.path.exists(constants.license_path):
        with open(constants.license_path, "rb") as stream:
            gm.License().set_license(stream)

        print("License set successfully.")
    else:
        print("\nWe do not ship any license with this example. " +
              "\nVisit the GroupDocs site to obtain either a temporary or permanent license. " +
              "\nLearn more about licensing at https://purchase.groupdocs.com/faqs/licensing. " +
              "\nLearn how to request a temporary license at https://purchase.groupdocs.com/temporary-license.")

if __name__ == "__main__":
    run()
```




### Applying Metered License

{{< alert style="info" >}}
You can also set [Metered](https://reference.groupdocs.com/python-net/metadata/groupdocs.metadata/metered) license as an alternative to license file. It is a new licensing mechanism that will be used along with existing licensing method. It is useful for the customers who want to be billed based on the usage of the API features. For more details, please refer to [Metered Licensing FAQ](https://purchase.groupdocs.com/faqs/licensing/metered) section.
{{< /alert >}}

Here are the simple steps to use the `Metered` class.

1.  Create an instance of `Metered` class.
2.  Pass public & private keys to `setMeteredKey` method.
3.  Do processing (perform task).
4.  call method `getConsumptionQuantity` of the `Metered` class.
5.  It will return the amount/quantity of API requests that you have consumed so far.
6.  call method `getConsumptionCredit` of the `Metered` class (Since version 19.5).
7.  It will return the credit that you have consumed so far.

Following is the sample code demonstrating how to use `Metered` class.

```python
import groupdocs.metadata as gm

def run():
    public_key = "*****"  # Your public key
    private_key = "*****"  # Your private key

    gm.Metered().set_metered_key(public_key, private_key)

    print("License set successfully.")

if __name__ == "__main__":
    run()


```


{{< alert style="info" >}}
Calling [License.setLicense](https://reference.groupdocs.com/metadata/python-net/com.groupdocs.metadata.licensing/License) multiple times is not harmful but simply wastes processor time. It is called once when the application starts.
{{< /alert >}}