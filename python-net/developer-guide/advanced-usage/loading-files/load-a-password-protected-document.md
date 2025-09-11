---
id: load-a-password-protected-document
url: metadata/python-net/load-a-password-protected-document
title: Load a password-protected document
weight: 4
description: "This example demonstrates how to load a password-protected document using GroupDocs.Metadata for Python via .NET."
keywords: load a password-protected document
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example demonstrates how to load a password-protected document.

**advanced_usage.loading_files.load_password_protected_document**


```python
def run():
    # Specify the password
    load_options = gm.LoadOptions()
    load_options.password = "123"

    # constants.protected_docx is an absolute or relative path to your document. Ex: r"C:\\Docs\\source.docx"
    with gm.Metadata(constants.protected_docx, load_options) as metadata:
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


