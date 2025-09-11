---
id: load-from-a-local-disk
url: metadata/python-net/load-from-a-local-disk
title: Load from a local disk
weight: 1
description: "Learn how to open a file from local disk using GroupDocs.Metadata for Python via .NET."
keywords: load file from local disk
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
The following example demonstrates how to load a file from a local disk using GroupDocs.Metadata for Python via .NET.

**advanced_usage.loading_files.load_from_local_disk**


```python
def run():
    # Absolute or relative path to your document, e.g. r"C:\\Docs\\source.one"
    input_path = r"C:\\Docs\\source.one"

    with gm.Metadata(input_path) as metadata:
        # Extract, edit or remove metadata here
        # ...
        pass
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


