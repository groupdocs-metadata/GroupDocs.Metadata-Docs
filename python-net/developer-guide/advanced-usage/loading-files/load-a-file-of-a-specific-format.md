---
id: load-a-file-of-a-specific-format
url: metadata/python-net/load-a-file-of-a-specific-format
title: Load a file of a specific format
weight: 3
description: "This example demonstrates how to load a file of a particular format using GroupDocs.Metadata for Python via .NET."
keywords: load a file, specific format
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example demonstrates how to load a file of some particular format.

**advanced_usage.loading_files.loading_file_of_specific_format**


```python
def run():
    # Explicitly specifying the format of a file to load can spare time on detecting the format
    load_options = gm.LoadOptions(gm.common.FileFormat.SPREADSHEET)

    # constants.input_xls is an absolute or relative path to your document. Ex: r"C:\\Docs\\source.xls"
    with gm.Metadata(constants.input_xls, load_options) as metadata:
        root = metadata.get_root_package()

        # Use format-specific properties to extract or edit metadata
        print(root.document_properties.author)

        # ...
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


