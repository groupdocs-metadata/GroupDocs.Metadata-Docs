---
id: system-requirements
url: metadata/python-net/system-requirements
title: System Requirements
weight: 3
description: "System requirements for GroupDocs.Metadata for Python via .NET — supported operating systems, Python versions, pip, and the Linux packages it needs."
keywords: GroupDocs.Metadata for Python via .NET, system requirements, Windows, Linux, macOS, Python 3.5, Python 3.14, glibc, ICU, fontconfig, libgdiplus
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
toc: True
---

{{< alert style="info" >}}
GroupDocs.Metadata for Python via .NET ships as a self-contained wheel that bundles the .NET runtime it needs. No Microsoft Office, Adobe software or Mono install is required.
{{< /alert >}}

## Supported Operating Systems

### Windows

- Windows 10 and Windows 11 (x64)
- Windows Server 2012 and later (x64)

The Windows wheel is 64-bit only: use a 64-bit Python. There is no 32-bit (x86) wheel.

### Linux

- Any **x86-64** distribution with **glibc 2.27 or newer** — for example Ubuntu 18.04+, Debian 10+, RHEL 8+. The embedded .NET runtime needs glibc 2.27, and the wheel's `manylinux_2_27_x86_64` tag says so, so pip refuses an older system.

### macOS

- macOS 12 (Monterey) and later, **Intel** (x86_64) and **Apple Silicon** (arm64 / M-series). Every binary of the embedded .NET runtime requires macOS 12, and the wheels are tagged `macosx_12_0_*` accordingly. Microsoft supports .NET 10 on macOS 14 and later.

## Python Version

GroupDocs.Metadata for Python via .NET supports every Python release from **3.5** through **3.14** (`python_requires = ">=3.5,<3.15"`). Download Python from the [official website](https://www.python.org/downloads/).

## Package Manager

The library is distributed on [PyPI](https://pypi.org/project/groupdocs-metadata-net/) as **`groupdocs-metadata-net`**, in four platform-specific wheels per release:

| Platform | Wheel suffix |
|---|---|
| Windows x86-64 | `win_amd64` |
| Linux x86-64 | `manylinux_2_27_x86_64` |
| macOS Apple Silicon (ARM64) | `macosx_12_0_arm64` |
| macOS Intel (x86-64) | `macosx_12_0_x86_64` |

`pip` 20.3 or newer picks the right wheel for your platform; older versions do not recognise these tags, so upgrade with `python -m pip install --upgrade pip`. On an Intel Mac, a Python built against a pre-11 macOS SDK reports its system as macOS 10.16 — there use pip 24.1 or newer, or run `SYSTEM_VERSION_COMPAT=0 pip install groupdocs-metadata-net`.

## Platform Dependencies

{{< alert style="info" >}}
**`libgdiplus` is not required.** Since version 26.9 every example and test passes in a Linux container that has no `libgdiplus`. If an existing image or script installs `libgdiplus` or `mono-libgdiplus` for GroupDocs.Metadata, you can remove it. See [Do I need libgdiplus?]({{< ref "/metadata/python-net/getting-started/troubleshooting/how-to-install-libgdiplus.md" >}}).
{{< /alert >}}

### Linux

The engine needs **ICU** and **fontconfig**:

```bash
# Debian / Ubuntu
sudo apt-get install -y libicu-dev libfontconfig1

# Fedora / RHEL / Rocky
sudo dnf install -y libicu fontconfig
```

- Without ICU the runtime cannot start: the first call aborts the Python process with "Couldn't find a valid ICU package". Do not set `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1` to work around it.
- Without fontconfig, opening presentations (PPTX, PPT) and exporting metadata to XLSX fail, because the bundled SkiaSharp and Aspose.Slides libraries load `libfontconfig.so.1`.
- No other fonts are needed: GroupDocs.Metadata reads and writes metadata; it does not render pages.

### macOS

No additional packages are required.

### Windows

No additional system libraries are required.
