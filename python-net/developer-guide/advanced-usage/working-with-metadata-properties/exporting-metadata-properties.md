---
id: exporting-metadata-properties
url: metadata/python-net/exporting-metadata-properties
title: Exporting metadata properties
weight: 1
description: "Export metadata properties to an Excel workbook, CSV, JSON or XML using GroupDocs.Metadata for Python via .NET."
keywords: export metadata properties, export metadata to excel, csv, json, xml
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example demonstrates how to export metadata properties to an Excel workbook. The `ExportManager` constructor takes a list of `MetadataProperty` objects, so collect the properties you want (here, the whole tree via `find_properties`) and wrap the result in `list(...)`. `ExportFormat` also supports `CSV`, `JSON` and `XML`.

{{< tabs "exporting-metadata-properties">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.export import ExportManager, ExportFormat


def exporting_metadata_properties():
    with Metadata("input.pdf") as metadata:
        # Collect the whole metadata tree as a list of properties
        properties = list(metadata.find_properties(lambda p: True))

        # Export them to an Excel workbook
        ExportManager(properties).export("export.xlsx", ExportFormat.XLSX)
        print(f"Exported {len(properties)} properties to export.xlsx")


if __name__ == "__main__":
    exporting_metadata_properties()
```
{{< /tab >}}
{{< tab "input.pdf" >}}
{{< tab-text >}}
`input.pdf` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/working-with-metadata-properties/exporting-metadata-properties/input.pdf) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "export.xlsx" >}}  
```text
Binary file (XLSX, 9 KB)
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/working-with-metadata-properties/exporting-metadata-properties/exporting_metadata_properties/export.xlsx)
{{< /tab >}}
{{< /tabs >}}

## See also

- [Extracting property values]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-properties/extracting-property-values.md" >}})
- [Working with metadata properties]({{< ref "metadata/python-net/developer-guide/advanced-usage/working-with-metadata-properties/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
