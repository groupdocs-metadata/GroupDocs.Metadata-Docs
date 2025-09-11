---
id: exporting-metadata-properties
url: metadata/python-net/exporting-metadata-properties
title: Exporting metadata properties
weight: 1
description: "Export metadata properties to an Excel workbook using GroupDocs.Metadata for Python via .NET."
keywords: export metadata properties, export metadata properties to an Excel workbook  
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example demonstrates how to export metadata properties to an Excel workbook. For more information please refer to the `ExportManager` class.

**advanced_usage.exporting_metadata_properties**


```python
def run():
    with gm.Metadata(r"D:\\input.doc") as metadata:
        root = metadata.get_root_package()
        if root is not None:
            # Initialize the export manager with the root metadata package to export the whole metadata tree
            manager = gm.export.ExportManager(root)
            manager.export(r"D:\\export.xls", gm.export.ExportFormat.XLS)
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


