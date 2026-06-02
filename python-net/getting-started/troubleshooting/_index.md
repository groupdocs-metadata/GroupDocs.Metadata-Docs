---
id: troubleshooting
url: metadata/python-net/getting-started/troubleshooting
title: Troubleshooting
weight: 7
description: "Common issues you may face while processing files with GroupDocs.Metadata for Python via .NET, and how to solve them."
keywords: GroupDocs.Metadata, troubleshooting, known issues, errors, libgdiplus, evaluation
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
This section describes issues you may face while processing files with GroupDocs.Metadata for Python via .NET, and their solutions.

## Common issues

**`GroupDocsMetadataException: Could not save the file. Evaluation only.`** — you are running unlicensed and the trial mode blocks saving. Apply a license or set the `GROUPDOCS_LIC_PATH` environment variable. See [Evaluation Limitations and Licensing]({{< ref "/metadata/python-net/getting-started/evaluation-limitations-and-licensing.md" >}}).

**`DocumentProtectedException`** — the document is password-protected. Provide the password through `LoadOptions`:

```python
from groupdocs.metadata import Metadata
from groupdocs.metadata.options import LoadOptions

load_options = LoadOptions()
load_options.password = "your-password"
with Metadata("protected.docx", load_options) as metadata:
    ...
```

**Only a few properties are returned** — without a license the API reads only the first few properties of each metadata package. Apply a license to read everything.

**`System.Drawing.Common is not supported` / `Gdip` errors (Linux/macOS)** — install the `libgdiplus` library. See [How to install libgdiplus]({{< ref "/metadata/python-net/getting-started/troubleshooting/how-to-install-libgdiplus.md" >}}).

**`DOTNET_SYSTEM_GLOBALIZATION_INVARIANT` errors (Linux)** — do NOT set this variable; install ICU instead (`sudo apt install libicu-dev`).

**`TypeLoadException` after upgrading** — reinstall the package: `pip install --force-reinstall groupdocs-metadata-net`.

## Related articles

For details, please refer to the following pages:

- [How to install the libgdiplus library]({{< ref "metadata/python-net/getting-started/troubleshooting/how-to-install-libgdiplus.md" >}}) — resolve `System.Drawing` / GDI+ errors on Linux and macOS.
- [Evaluation limitations and licensing]({{< ref "metadata/python-net/getting-started/evaluation-limitations-and-licensing.md" >}}) — clear the "Evaluation only" save error and the property-read cap.
- [System requirements]({{< ref "metadata/python-net/getting-started/system-requirements.md" >}}) — required system libraries and fonts.
