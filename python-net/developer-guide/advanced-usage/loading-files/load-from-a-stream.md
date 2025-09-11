---
id: load-from-a-stream
url: metadata/python-net/load-from-a-stream
title: Load from a stream
weight: 2
description: "This example demonstrates how to load a file from a stream using GroupDocs.Metadata for Python via .NET."
keywords: load a file from a stream
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example demonstrates how to load a file from a stream.

**advanced_usage.loading_files.load_from_stream**


```python
def run():
    # constants.input_doc is an absolute or relative path to your document. Ex: r"C:\\Docs\\source.doc"
    with open(constants.input_doc, 'rb') as stream:
        with gm.Metadata(stream) as metadata:
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


