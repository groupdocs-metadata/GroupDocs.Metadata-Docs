---
id: save-a-modified-file-to-a-stream
url: metadata/python-net/save-a-modified-file-to-a-stream
title: Save a modified file to a stream
weight: 3
description: "This article shows how to save a file to the specified stream using GroupDocs.Metadata for Python via .NET."
keywords: save a file to the specified stream
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example shows how to save a file to the specified stream.

**advanced_usage.saving_files.save_file_to_specified_stream**


```python
def run():
    stream = io.BytesIO()
    # constants.input_png is an absolute or relative path to your document. Ex: r"C:\\Docs\\test.png"
    with gm.Metadata(constants.input_png) as metadata:
        # Edit or remove metadata here
        # ...

        metadata.save(stream)
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


