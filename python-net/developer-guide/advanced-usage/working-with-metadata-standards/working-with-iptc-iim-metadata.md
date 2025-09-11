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

The IPTC Information Interchange Model (IIM) is a set of metadata properties that can be applied to text, images, and other media types. The standard also defines a complex data structure that is utilized to store the properties. The IPTC IIM was developed by the International Press Telecommunications Council (IPTC) to expedite the international exchange of news among newspapers and news agencies. Nowadays it's also commonly used by photographers. IPTC IIM metadata can be embedded into a variety of image formats such as JPEG, TIFF, etc.

{{< alert style="info" >}}For more information on the IPTC IIM standard please refer to https://www.iptc.org/std/IIM/4.2/specification/IIMV4.2.pdf.{{< /alert >}}

## Reading basic IPTC IIM properties

To access IPTC metadata in a file of any supported format, GroupDocs.Metadata provides the `IIptc.iptc_package` property. The following are the steps to read IPTC metadata:

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains IPTC metadata
2.  Extract the IPTC metadata package using the `IIptc.iptc_package` property
3.  Read properties of the `IptcApplicationRecord` and `IptcEnvelopeRecord` class instances

The following code snippet gets IPTC properties of a JPEG image and displays them on the screen. 

**advanced_usage.working_with_metadata_standards.iptc.read_basic_iptc_properties**


```python
def run():
    with gm.Metadata(constants.jpeg_with_iptc) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "iptc_package") and root.iptc_package is not None:
            if root.iptc_package.envelope_record is not None:
                print(root.iptc_package.envelope_record.date_sent)
                print(root.iptc_package.envelope_record.destination)
                print(root.iptc_package.envelope_record.file_format)
                print(root.iptc_package.envelope_record.file_format_version)

                # ...

            if root.iptc_package.application_record is not None:
                print(root.iptc_package.application_record.headline)
                print(root.iptc_package.application_record.by_line)
                print(root.iptc_package.application_record.by_line_title)
                print(root.iptc_package.application_record.caption_abstract)
                print(root.iptc_package.application_record.city)
                print(root.iptc_package.application_record.date_created)
                print(root.iptc_package.application_record.release_date)

                # ...
```


## Reading all IPTC IIM datasets

In some cases, it's necessary to read all IPTC datasets (metadata properties) from a file, including custom ones. To achieve this the GroupDocs.Metadata API provides direct access to the IPTC datasets extracted from a file.

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains IPTC metadata
2.  Extract the IPTC metadata package using the `IIptc.iptc_package` property
3.  Iterate through all IPTC datasets

**advanced_usage.working_with_metadata_standards.iptc.read_iptc_datasets**


```python
def run():
    with gm.Metadata(constants.psd_with_iptc) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "iptc_package") and root.iptc_package is not None:
            for dataset in root.iptc_package.to_dataset_list():
                print(dataset.record_number)
                print(dataset.dataset_number)
                print(dataset.alternative_name)
                if dataset.value.type == gm.common.MetadataPropertyType.PROPERTY_VALUE_ARRAY:
                    for val in dataset.value.to_array(gm.common.PropertyValue):
                        print(f"{val}, ", end="")
                    print()
                else:
                    print(dataset.value)
```


## Updating IPTC IIM properties

The GroupDocs.Metadata API facilitates the user to update IPTC metadata in a convenient way - using the `IptcRecordSet` class properties. Follow the below steps to update IPTC metadata in a file of any supported format.

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains IPTC metadata
2.  Extract the IPTC metadata package using the `IIptc.iptc_package` property
3.  Assign values to desired IPTC properties
4.  [Save]({{< ref "metadata/python-net/developer-guide/advanced-usage/saving-files/_index.md" >}}) the changes

**advanced_usage.working_with_metadata_standards.iptc.update_iptc_properties**


```python
def run():
    with gm.Metadata(constants.input_jpeg) as metadata:
        root = metadata.get_root_package()
        # Set the IPTC package if it's missing
        if not hasattr(root, "iptc_package") or root.iptc_package is None:
            root.iptc_package = gm.standards.iptc.IptcRecordSet()

        if root.iptc_package.envelope_record is None:
            root.iptc_package.envelope_record = gm.standards.iptc.IptcEnvelopeRecord()
        root.iptc_package.envelope_record.date_sent = datetime.now()
        root.iptc_package.envelope_record.product_id = str(uuid.uuid4())

        # ...

        if root.iptc_package.application_record is None:
            root.iptc_package.application_record = gm.standards.iptc.IptcApplicationRecord()
        root.iptc_package.application_record.by_line = "GroupDocs"
        root.iptc_package.application_record.headline = "test"
        root.iptc_package.application_record.by_line_title = "code sample"
        root.iptc_package.application_record.release_date = date.today()

        # ...

        metadata.save(constants.output_jpeg)
```


## Adding or updating custom IPTC IIM datasets

The GroupDocs.Metadata API allows adding or updating custom datasets in an IPTC package.

1.  [Load]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a file that contains IPTC metadata
2.  Extract the IPTC metadata package using the `IIptc.iptc_package` property
3.  Set the IPTC package if it's missing
4.  Add any number of custom datasets to the package
5.  [Save]({{< ref "metadata/python-net/developer-guide/advanced-usage/saving-files/_index.md" >}}) the changes

**advanced_usage.working_with_metadata_standards.iptc.set_custom_iptc_dataset**


```python
def run():
    with gm.Metadata(constants.psd_with_iptc) as metadata:
        root = metadata.get_root_package()
        if not hasattr(root, "iptc_package") or root.iptc_package is None:
            root.iptc_package = gm.standards.iptc.IptcRecordSet()

        # Add a known property using the DataSet API
        root.iptc_package.set(gm.standards.iptc.IptcDataSet(int(gm.standards.iptc.IptcRecordType.APPLICATION_RECORD), int(gm.standards.iptc.IptcApplicationRecordDataSet.BYLINE_TITLE), "test code sample"))

        # Add a custom IPTC DataSet
        root.iptc_package.set(gm.standards.iptc.IptcDataSet(255, 255, bytes([1, 2, 3])))

        metadata.save(constants.output_psd)
```


## Adding repeatable IPTC IIM datasets

According to the IPTC IIM specification some datasets can be added to a record multiple times. The code snippet below demonstrates how to add a repeatable dataset to a record.

**advanced_usage.working_with_metadata_standards.iptc.add_repeatable_iptc_dataset**


```python
def run():
    with gm.Metadata(constants.psd_with_iptc) as metadata:
        root = metadata.get_root_package()
        if not hasattr(root, "iptc_package") or root.iptc_package is None:
            root.iptc_package = gm.standards.iptc.IptcRecordSet()
        root.iptc_package.add(gm.standards.iptc.IptcDataSet(int(gm.standards.iptc.IptcRecordType.APPLICATION_RECORD), int(gm.standards.iptc.IptcApplicationRecordDataSet.KEYWORDS), "keyword 1"))
        root.iptc_package.add(gm.standards.iptc.IptcDataSet(int(gm.standards.iptc.IptcRecordType.APPLICATION_RECORD), int(gm.standards.iptc.IptcApplicationRecordDataSet.KEYWORDS), "keyword 2"))
        root.iptc_package.add(gm.standards.iptc.IptcDataSet(int(gm.standards.iptc.IptcRecordType.APPLICATION_RECORD), int(gm.standards.iptc.IptcApplicationRecordDataSet.KEYWORDS), "keyword 3"))
        metadata.save(constants.output_psd)

    # Check the output file
    with gm.Metadata(constants.output_psd) as metadata:
        root = metadata.get_root_package()
        keywords_property = root.iptc_package.application_record[int(gm.standards.iptc.IptcApplicationRecordDataSet.KEYWORDS)]
        for value in keywords_property.value.to_array(gm.common.PropertyValue):
            print(value)
```


## Removing IPTC IIM metadata

To remove the IPTC package from a file just assign `None` to the `IIptc.iptc_package` property. The code sample below shows how to remove IPTC metadata from a file.

**advanced_usage.working_with_metadata_standards.iptc.remove_iptc_metadata**


```python
def run():
    with gm.Metadata(constants.jpeg_with_iptc) as metadata:
        root = metadata.get_root_package()
        if hasattr(root, "iptc_package"):
            root.iptc_package = None
            metadata.save(constants.output_jpeg)
```


## More resources
### GitHub examples
You may easily run the code above and see the feature in action in our GitHub examples:
*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)    
*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App
Along with full featured Python via .NET library we provide simple, but powerful free Apps.
You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).


