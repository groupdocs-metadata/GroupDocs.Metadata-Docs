---
id: troubleshooting
url: metadata/python-net/getting-started/troubleshooting
title: Troubleshooting
weight: 7
description: "Common issues you may face while processing files with GroupDocs.Metadata for Python via .NET, and how to solve them."
keywords: GroupDocs.Metadata, troubleshooting, known issues, errors, ICU, fontconfig, libgdiplus, evaluation, pip
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

**`DllNotFoundException` naming `libSkiaSharp` or `libaspose.slides.drawing.capi…`, or `libfontconfig.so.1: cannot open shared object file` (Linux)** — fontconfig is missing. Opening presentations and exporting to XLSX need it: `sudo apt-get install libfontconfig1`.

**The process aborts with "Couldn't find a valid ICU package", or `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT` errors (Linux)** — install ICU (`sudo apt-get install libicu-dev`), and do NOT set `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT`.

**`libgdiplus` / `Gdip` errors (Linux/macOS)** — not expected: since 26.9 `libgdiplus` is not needed. See [Do I need libgdiplus?]({{< ref "/metadata/python-net/getting-started/troubleshooting/how-to-install-libgdiplus.md" >}}).

**`is not a supported wheel on this platform`, `No matching distribution found`, or pip installs a version older than 26.9 (Linux/macOS)** — the system is older than the wheel needs (glibc 2.27, macOS 12), or pip is older than 20.3: run `python -m pip install --upgrade pip`. Versions up to 26.7 were tagged for older systems, so an unpinned install falls back to them. On an Intel Mac, a Python built against an old SDK reports macOS 10.16 — use pip 24.1+ or `SYSTEM_VERSION_COMPAT=0 pip install groupdocs-metadata-net`.

**`Access to the path … is denied` or `Read-only file system` when opening a file** — `Metadata(path)` opens the file for writing, so a read-only file fails. Open it as a stream instead:

```python
with open("readonly.docx", "rb") as stream:
    with Metadata(stream) as metadata:
        ...
```

**`The filename or extension is too long` when opening a presentation (Windows)** — the package is installed so deep that its native library's path exceeds 260 characters (roughly, a virtual environment path longer than 170 characters). Install it closer to the drive root.

**`TypeLoadException` after upgrading** — reinstall the package: `pip install --force-reinstall groupdocs-metadata-net`.

## Related articles

For details, please refer to the following pages:

- [Do I need libgdiplus?]({{< ref "metadata/python-net/getting-started/troubleshooting/how-to-install-libgdiplus.md" >}}) — no, since 26.9; what to do if a GDI+ error still appears.
- [Evaluation limitations and licensing]({{< ref "metadata/python-net/getting-started/evaluation-limitations-and-licensing.md" >}}) — clear the "Evaluation only" save error and the property-read cap.
- [System requirements]({{< ref "metadata/python-net/getting-started/system-requirements.md" >}}) — supported systems, pip, and the Linux packages (ICU, fontconfig).
