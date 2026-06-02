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
This example demonstrates how to load a password-protected document. Provide the password through `LoadOptions`. If the password is wrong, a `DocumentProtectedException` is raised.

{{< tabs "load-a-password-protected-document">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.options import LoadOptions


def load_password_protected_document():
    # Specify the password
    load_options = LoadOptions()
    load_options.password = "123"

    with Metadata("protected.docx", load_options) as metadata:
        # Extract, edit or remove metadata here
        print(f"Opened protected {metadata.file_format} document")


if __name__ == "__main__":
    load_password_protected_document()
```
{{< /tab >}}
{{< tab "protected.docx" >}}
{{< tab-text >}}
`protected.docx` is the sample file used in this example (password: `123`). Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/loading-files/load-a-password-protected-document/protected.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "load-password-protected-document.txt" >}}  
```text
Opened protected 3 document
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/loading-files/load-a-password-protected-document/load_password_protected_document/load-password-protected-document.txt)
{{< /tab >}}
{{< /tabs >}}

## See also

- [Load a file of a specific format]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/load-a-file-of-a-specific-format.md" >}})
- [Loading files]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
