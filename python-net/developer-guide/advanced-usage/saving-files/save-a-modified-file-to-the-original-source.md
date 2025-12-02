---
id: save-a-modified-file-to-the-original-source
url: metadata/python-net/save-a-modified-file-to-the-original-source
title: Save a modified file to the original source
weight: 1
description: "This article shows how to save the modified content to the underlying source using GroupDocs.Metadata for Python via .NET."
keywords: how to save the modified content to the underlying source
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example shows how to save the modified content to the underlying source.

**advanced_usage.saving_files.save_file_to_original_source**


```python
def run():
    # constants.output_ppt is an absolute or relative path to your document. Ex: r"C:\\Docs\\test.ppt"
    with gm.Metadata(constants.output_ppt) as metadata:
        # Edit or remove metadata here
        # ...

        # Saves the document to the underlying source (stream or file)
        metadata.save()
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


