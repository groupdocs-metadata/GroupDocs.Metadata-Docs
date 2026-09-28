---
id: how-to-install-libgdiplus
url: metadata/python-net/getting-started/troubleshooting/how-to-install-libgdiplus
title: Do I need libgdiplus?
weight: 1
description: "GroupDocs.Metadata for Python via .NET does not need libgdiplus on Linux or macOS since version 26.9. What Linux does need, and what to do if a libgdiplus error still appears."
keywords: libgdiplus, mono-libgdiplus, GroupDocs.Metadata, GDI+, System.Drawing, fontconfig, ICU, Linux, macOS, Docker
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---

**No.** Since version 26.9, GroupDocs.Metadata for Python via .NET does not need `libgdiplus` on Linux or `mono-libgdiplus` on macOS. Every example and test passes in a Linux container with no `libgdiplus` installed, including image metadata, sanitizing and exporting to Excel.

If an older version of this guide, a Dockerfile or a CI script installs `libgdiplus` for GroupDocs.Metadata, you can remove it. What Linux *does* need is ICU and fontconfig — see [System Requirements]({{< ref "/metadata/python-net/getting-started/system-requirements.md" >}}):

```bash
sudo apt-get install -y libicu-dev libfontconfig1
```

Windows never needed `libgdiplus`: GDI+ is part of the operating system.

## If a libgdiplus error still appears

An error such as `DllNotFoundException: Unable to load shared library 'libgdiplus'` or `The type initializer for 'Gdip' threw an exception` is not expected with 26.9 or later. Check that you are running a current version:

```bash
groupdocs-metadata --version
python -m pip show groupdocs-metadata-net
```

If you are on 26.9 or later and still see the error, install the library as a workaround and [report the document](https://forum.groupdocs.com/c/metadata/) that triggered it:

**Debian / Ubuntu**

```bash
sudo apt-get update
sudo apt-get install -y libgdiplus
```

**Red Hat / CentOS / Rocky** (from the EPEL repository)

```bash
sudo yum install -y epel-release
sudo yum install -y libgdiplus
```

**macOS**

```bash
brew install mono-libgdiplus
```

To confirm that the library is visible to the loader on Linux:

```bash
ldconfig -p | grep libgdiplus
```
