---
id: how-to-install-libgdiplus
url: metadata/python-net/getting-started/troubleshooting/how-to-install-libgdiplus
title: How to install the libgdiplus library
weight: 1
description: "Install libgdiplus for GroupDocs.Metadata for Python via .NET. Resolves image processing issues on Ubuntu, CentOS, and macOS."
keywords: libgdiplus, GroupDocs.Metadata, GDI+, System.Drawing, Ubuntu, CentOS, macOS
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---

## Overview

If you process images (or documents that contain images) with GroupDocs.Metadata for Python via .NET on Linux or macOS and hit errors related to `System.Drawing` or GDI+, you are likely missing the `libgdiplus` library. This library provides the GDI+-compatible API that .NET's `System.Drawing` relies on.

This guide walks you through installing `libgdiplus` on various operating systems.

## Ubuntu / Debian-based systems

Update the package list and install `libgdiplus`:

```bash
sudo apt-get update
sudo apt-get install -y libgdiplus
```

Create a symlink if required for compatibility:

```bash
sudo ln -s /usr/lib/libgdiplus.so /usr/lib/libgdiplus.so.0
```

## Red Hat / CentOS-based systems

Enable the EPEL repository and install `libgdiplus`:

```bash
sudo yum install epel-release
sudo yum install -y libgdiplus
```

## macOS

Install `libgdiplus` via Homebrew:

```bash
brew install mono-libgdiplus
```

## Windows

On Windows you usually do not need `libgdiplus`, as GDI+ functionality is built into the operating system.

## Verifying the installation

After installing, verify it is found:

```bash
ldconfig -p | grep libgdiplus
```

### Common issues

*   **"Cannot find libgdiplus.so"** — ensure the symbolic link exists; re-run the `ln -s` command above.
*   **"System.Drawing.Common is not supported" persists** — make sure your .NET runtime is up to date and that fonts are installed (`sudo apt install ttf-mscorefonts-installer fontconfig && sudo fc-cache -f`).
