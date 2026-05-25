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
This example demonstrates how to load a file from a binary stream. Pass any readable binary file-like object (an `open(..., "rb")` handle or an `io.BytesIO`) to the `Metadata` constructor.

{{< tabs "load-from-a-stream">}}
{{< tab "Python" >}}
```python
from groupdocs.metadata import Metadata


def load_from_stream():
    with open("input.docx", "rb") as stream:
        with Metadata(stream) as metadata:
            # Extract, edit or remove metadata here
            print(f"Loaded {metadata.file_format} from a stream")


if __name__ == "__main__":
    load_from_stream()
```
{{< /tab >}}
{{< tab "input.docx" >}}
{{< tab-text >}}
`input.docx` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/loading-files/load-from-a-stream/input.docx) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "load-from-stream.txt" >}}  
```text
Loaded 3 from a stream
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/loading-files/load-from-a-stream/load_from_stream/load-from-stream.txt)
{{< /tab >}}
{{< /tabs >}}

## See also

- [Load from a local disk]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/load-from-a-local-disk.md" >}})
- [Loading files]({{< ref "metadata/python-net/developer-guide/advanced-usage/loading-files/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
