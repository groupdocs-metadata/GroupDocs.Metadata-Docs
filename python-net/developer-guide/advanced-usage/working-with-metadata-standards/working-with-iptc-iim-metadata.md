---
id: working-with-iptc-iim-metadata
url: metadata/python-net/working-with-iptc-iim-metadata
title: Working with IPTC IIM metadata
weight: 2
description: "Access, read, update, and remove IPTC IIM metadata using GroupDocs.Metadata for Python via .NET."
keywords: IPTC, IIM, IPTC metadata
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
## What is IPTC IIM?

The IPTC Information Interchange Model (IIM) is a set of metadata properties that can be applied to text, images, and other media. The standard also defines a data structure used to store the properties. IPTC IIM was developed by the International Press Telecommunications Council (IPTC) to speed up the international exchange of news, and is now also commonly used by photographers. IPTC IIM metadata can be embedded into a variety of image formats such as JPEG and TIFF.

{{< alert style="info" >}}For more information on the IPTC IIM standard please refer to https://www.iptc.org/std/IIM/4.2/specification/IIMV4.2.pdf.{{< /alert >}}

## Reading basic IPTC IIM properties

To access IPTC metadata in a file of any supported format, GroupDocs.Metadata provides the `iptc_package` property on the root package, which exposes `envelope_record` and `application_record`.

{{< tabs "iptc-read-basic">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def read_basic_iptc_properties():
    with Metadata("iptc.jpg") as metadata:
        root = metadata.get_root_package()
        iptc = getattr(root, "iptc_package", None)
        if iptc is not None:
            # The envelope record carries transmission-level fields
            if iptc.envelope_record is not None:
                print(iptc.envelope_record.date_sent)
                print(iptc.envelope_record.destination)
                print(iptc.envelope_record.file_format)

            # The application record carries the editorial fields
            if iptc.application_record is not None:
                print(iptc.application_record.headline)
                print(iptc.application_record.by_line)
                print(iptc.application_record.caption_abstract)
                print(iptc.application_record.city)
                print(iptc.application_record.date_created)


if __name__ == "__main__":
    read_basic_iptc_properties()
```
{{< /tab >}}
{{< tab "iptc.jpg" >}}
{{< tab-text >}}
`iptc.jpg` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata/iptc.jpg) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "read-basic-iptc-properties.txt" >}}  
```text
2019-09-21 00:00:00
None
8
test
None
None
None
2019-09-18 00:00:00
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata/read_basic_iptc_properties/read-basic-iptc-properties.txt)
{{< /tab >}}
{{< /tabs >}}

## Reading all IPTC IIM datasets

The API provides direct access to the IPTC datasets extracted from a file, including custom ones.

{{< tabs "iptc-read-datasets">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def read_iptc_datasets():
    with Metadata("iptc.psd") as metadata:
        root = metadata.get_root_package()
        iptc = getattr(root, "iptc_package", None)
        if iptc is not None:
            # to_data_set_list() returns every raw IPTC dataset
            for dataset in iptc.to_data_set_list():
                print(dataset.record_number)       # record (e.g. 1=envelope, 2=application)
                print(dataset.data_set_number)     # dataset id within the record
                print(dataset.alternative_name)    # human-readable name
                print(dataset.value.raw_value)     # underlying value


if __name__ == "__main__":
    read_iptc_datasets()
```
{{< /tab >}}
{{< tab "iptc.psd" >}}
{{< tab-text >}}
`iptc.psd` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata/iptc.psd) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "read-iptc-datasets.txt" >}}  
```text
1
90
CodedCharacterSet
GroupDocs.Metadata.Common.PropertyValue[]
2
0
RecordVersion
32
2
55
[TRUNCATED]
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata/read_iptc_datasets/read-iptc-datasets.txt)
{{< /tab >}}
{{< /tabs >}}

## Updating IPTC IIM properties

Update IPTC metadata using the `IptcRecordSet` properties. Create the records if they are missing.

{{< tabs "iptc-update">}}
{{< tab "Python" >}}
```python
import uuid
from datetime import date, datetime

from groupdocs.metadata import Metadata
from groupdocs.metadata.standards.iptc import IptcApplicationRecord, IptcEnvelopeRecord, IptcRecordSet


def update_iptc_properties():
    with Metadata("input.jpg") as metadata:
        root = metadata.get_root_package()
        # Create the IPTC record set if the image has none
        if getattr(root, "iptc_package", None) is None:
            root.iptc_package = IptcRecordSet()

        # Fill the envelope record (create it first if missing)
        if root.iptc_package.envelope_record is None:
            root.iptc_package.envelope_record = IptcEnvelopeRecord()
        root.iptc_package.envelope_record.date_sent = datetime.now()
        root.iptc_package.envelope_record.product_id = str(uuid.uuid4())

        # Fill the application record (create it first if missing)
        if root.iptc_package.application_record is None:
            root.iptc_package.application_record = IptcApplicationRecord()
        root.iptc_package.application_record.by_line = "GroupDocs"
        root.iptc_package.application_record.headline = "test"
        root.iptc_package.application_record.by_line_title = "code sample"
        root.iptc_package.application_record.release_date = date.today()

        # Persist the changes
        metadata.save("output.jpg")


if __name__ == "__main__":
    update_iptc_properties()
```
{{< /tab >}}
{{< tab "input.jpg" >}}
{{< tab-text >}}
`input.jpg` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata/input.jpg) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.jpg" >}}  
```text
Binary file (JPG, 885 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata/update_iptc_properties/output.jpg)
{{< /tab >}}
{{< /tabs >}}

## Adding or updating custom IPTC IIM datasets

The API allows adding or updating custom datasets in an IPTC package.

{{< tabs "iptc-set-custom">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.standards.iptc import (
    IptcApplicationRecordDataSet, IptcDataSet, IptcRecordSet, IptcRecordType,
)


def set_custom_iptc_dataset():
    with Metadata("iptc.psd") as metadata:
        root = metadata.get_root_package()
        if getattr(root, "iptc_package", None) is None:
            root.iptc_package = IptcRecordSet()

        # Add a known property using the DataSet API
        root.iptc_package.set(IptcDataSet(
            int(IptcRecordType.APPLICATION_RECORD),
            int(IptcApplicationRecordDataSet.BYLINE_TITLE),
            "test code sample",
        ))

        # Add a fully custom IPTC DataSet
        root.iptc_package.set(IptcDataSet(255, 255, bytes([1, 2, 3])))

        metadata.save("output.psd")


if __name__ == "__main__":
    set_custom_iptc_dataset()
```
{{< /tab >}}
{{< tab "iptc.psd" >}}
{{< tab-text >}}
`iptc.psd` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata/iptc.psd) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.psd" >}}  
```text
Binary file (PSD, 19.5 MB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata/set_custom_iptc_dataset/output.psd)
{{< /tab >}}
{{< /tabs >}}

## Removing IPTC IIM metadata

To remove the IPTC package from a file, assign `None` to the `iptc_package` property.

{{< tabs "iptc-remove">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def remove_iptc_metadata():
    with Metadata("iptc.jpg") as metadata:
        root = metadata.get_root_package()
        # Assigning None drops the entire IPTC package
        root.iptc_package = None
        metadata.save("output.jpg")


if __name__ == "__main__":
    remove_iptc_metadata()
```
{{< /tab >}}
{{< tab "iptc.jpg" >}}
{{< tab-text >}}
`iptc.jpg` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata/iptc.jpg) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "output.jpg" >}}  
```text
Binary file (JPG, 948 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-standards/working-with-iptc-iim-metadata/remove_iptc_metadata/output.jpg)
{{< /tab >}}
{{< /tabs >}}

## See also

- [Working with EXIF metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-exif-metadata.md" >}})
- [Working with XMP metadata]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata.md" >}})
- [Working with metadata standards]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-standards/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
