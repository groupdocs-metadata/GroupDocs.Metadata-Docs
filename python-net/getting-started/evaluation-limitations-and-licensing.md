---
id: evaluation-limitations-and-licensing
url: metadata/python-net/evaluation-limitations-and-licensing
title: Evaluation Limitations and Licensing
linkTitle: Licensing
weight: 5
description: "GroupDocs.Metadata for Python via .NET offers a Free Trial and a 30-day Temporary License for evaluation, plus several purchase plans."
keywords: free, free trial, evaluation, license, temporary license, metered
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
toc: True
---

To study the API quickly, GroupDocs.Metadata offers a Free Trial and a 30-day Temporary License for evaluation, along with several purchase plans.

{{< alert style="info" >}}GroupDocs.Metadata also works without a license in trial mode, with certain limitations.{{< /alert >}}

## Purchased License

After buying, apply the license file or include it as an embedded resource. The license needs to be set:

- Only once per application
- Before using any other GroupDocs.Metadata classes

## Evaluation version limitations

The evaluation download is identical to the purchased one — it simply becomes licensed when you apply a license. Without a license you will face the following limitations:

| API | Limitation |
| --- | --- |
| Document properties (PDF, Word, Excel, PowerPoint, Visio, etc.) | Only the first 5 properties can be read |
| XMP API | Only the first 2 XMP schemes can be read |
| EXIF API | GPS IFD and image thumbnail are unavailable; only the first 5 properties can be read |
| IPTC API | Only the first 5 properties can be read |
| ID3v2, Lyrics3, APEv2 tags | Only the first 5 properties can be read |
| QuickTime atoms | Only the first 5 atoms can be read |
| File open | A maximum of 15 files can be opened, otherwise the API throws an exception |
| File save | **Not supported in trial mode** — `save()` raises an "Evaluation only" exception |

## Licensing

The license file contains details such as the product name, the number of developers it is licensed to, and the subscription expiry date. It carries a digital signature, so don't modify the file — even an inadvertent extra line break will invalidate it. Set a license before using the API to avoid the evaluation limitations.

The license can be loaded from a file or from a stream.

### Applying license from a file

{{< tabs "set-license-from-file">}}
{{< tab "Python" >}}
```python
import os

from groupdocs.metadata import License


def set_license_from_file():
    # Resolve the license path relative to the current working directory
    license_path = os.path.abspath("./GroupDocs.Metadata.lic")
    if os.path.exists(license_path):
        # Apply the license once, before using any other Metadata API
        license = License()
        license.set_license(license_path)
        print("License set successfully.")
    else:
        print("License file not found; running in evaluation mode.")


if __name__ == "__main__":
    set_license_from_file()
```
{{< /tab >}}
{{< /tabs >}}

### Applying license from a stream

{{< tabs "set-license-from-stream">}}
{{< tab "Python" >}}
```python
import os

from groupdocs.metadata import License


def set_license_from_stream():
    license_path = os.path.abspath("./GroupDocs.Metadata.lic")
    if os.path.exists(license_path):
        # set_license also accepts a readable binary stream
        with open(license_path, "rb") as stream:
            License().set_license(stream)
        print("License set successfully.")
    else:
        print("License file not found; running in evaluation mode.")


if __name__ == "__main__":
    set_license_from_stream()
```
{{< /tab >}}
{{< /tabs >}}

### Applying a Metered License

{{< alert style="info" >}}
You can also set a [Metered](https://reference.groupdocs.com/metadata/python-net/) license as an alternative to a license file. It is a licensing mechanism that bills based on usage. For more details, please refer to the [Metered Licensing FAQ](https://purchase.groupdocs.com/faqs/licensing/metered).
{{< /alert >}}

The simple steps to use the `Metered` class:

1.  Create an instance of the `Metered` class.
2.  Pass the public and private keys to the `set_metered_key` method.
3.  Perform your processing.
4.  Call `Metered.get_consumption_quantity()` to get the amount of API requests consumed so far.
5.  Call `Metered.get_consumption_credit()` to get the credit consumed so far.

{{< tabs "set-metered-license">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metered


def set_metered_license():
    public_key = "*****"  # Your public key
    private_key = "*****"  # Your private key

    # Guard the sample so it doesn't call the API with placeholder keys
    if "*" in public_key or "*" in private_key:
        print("Provide your real metered keys to activate metered licensing.")
        return

    # Activate metered (pay-as-you-go) billing for this process
    Metered().set_metered_key(public_key, private_key)
    print("Metered license set successfully.")


if __name__ == "__main__":
    set_metered_license()
```
{{< /tab >}}
{{< /tabs >}}

{{< alert style="info" >}}
Calling `License.set_license` multiple times is not harmful but simply wastes processor time. Call it once when the application starts.
{{< /alert >}}
