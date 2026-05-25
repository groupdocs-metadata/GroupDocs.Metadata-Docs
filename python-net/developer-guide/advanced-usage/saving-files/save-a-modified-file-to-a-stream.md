---
id: save-a-modified-file-to-a-stream
url: metadata/python-net/save-a-modified-file-to-a-stream
title: Save a modified file to a stream
weight: 3
description: "This article shows how to save a file to a specified stream using GroupDocs.Metadata for Python via .NET."
keywords: save a file to the specified stream
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This example shows how to save a modified file to a stream. Pass any writable binary file-like object (for example an `io.BytesIO`) to the `save` method; the wrapper writes the result into it.

{{< tabs "save-a-modified-file-to-a-stream">}}
{{< tab "Python" >}}
```python
import io

from groupdocs.metadata import Metadata


def save_file_to_specified_stream():
    stream = io.BytesIO()
    with Metadata("input.png") as metadata:
        # Edit or remove metadata here
        metadata.sanitize()
        metadata.save(stream)

    print(f"Saved {stream.getbuffer().nbytes} bytes to the stream")


if __name__ == "__main__":
    save_file_to_specified_stream()
```
{{< /tab >}}
{{< tab "input.png" >}}
{{< tab-text >}}
`input.png` is the sample file used in this example. Click [here](/metadata/python-net/_sample_files/developer-guide/advanced-usage/saving-files/save-a-modified-file-to-a-stream/input.png) to download it.
{{< /tab-text >}}
{{< /tab >}}
{{< tab "save-file-to-specified-stream.txt" >}}  
```text
Saved 51759 bytes to the stream
```
[Download full output](/metadata/python-net/_output_files/developer-guide/advanced-usage/saving-files/save-a-modified-file-to-a-stream/save_file_to_specified_stream/save-file-to-specified-stream.txt)
{{< /tab >}}
{{< /tabs >}}

## See also

- [Save a modified file to a specified location]({{< ref "metadata/python-net/developer-guide/advanced-usage/saving-files/save-a-modified-file-to-a-specified-location.md" >}})
- [Saving files]({{< ref "metadata/python-net/developer-guide/advanced-usage/saving-files/_index.md" >}})

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
