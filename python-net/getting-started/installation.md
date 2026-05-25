---
id: installation
url: metadata/python-net/installation
title: Installation
weight: 4
description: "Install GroupDocs.Metadata for Python via .NET from PyPI, pin it in requirements.txt, or install a pre-downloaded wheel for offline environments — then verify the installation."
keywords: install, installation, pip, pypi, requirements.txt, offline install, wheel, whl, groupdocs-metadata-net, python
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
toc: True
---

GroupDocs.Metadata for Python via .NET is distributed as a self-contained wheel that bundles the embedded .NET runtime, so **no additional software is required**. A single `py3-none-{platform}` wheel works across Python **3.5 – 3.14** on Windows, Linux, and macOS (Intel and Apple Silicon).

{{< alert style="info" >}}
Before you install, review the [System Requirements]({{< ref "/metadata/python-net/getting-started/system-requirements.md" >}}). On Linux and macOS, image and document-image processing additionally needs the `libgdiplus` native library — see the system-requirements page for the exact packages.
{{< /alert >}}

## Install Package from PyPI

All packages are hosted at [PyPI](https://pypi.org/project/groupdocs-metadata-net/). Install the latest version with `pip`:

{{< tabs "install-from-pypi">}}
{{< tab "Windows" >}}
```ps
py -m pip install groupdocs-metadata-net
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m pip install groupdocs-metadata-net
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m pip install groupdocs-metadata-net
```
{{< /tab >}}
{{< /tabs >}}

To upgrade an existing installation to the newest release, add the `--upgrade` flag:

```bash
python3 -m pip install --upgrade groupdocs-metadata-net
```

{{< alert style="info" >}}
Using a [virtual environment](https://docs.python.org/3/library/venv.html) is recommended so the package and its dependencies stay isolated from your system Python. See the [Quick Start Guide]({{< ref "/metadata/python-net/getting-started/quick-start-guide.md" >}}) for the `venv` setup steps.
{{< /alert >}}

## Add the Package to `requirements.txt`

For reproducible builds, pin the version in your project's `requirements.txt`:

```text
groupdocs-metadata-net==26.5
```

Then install every dependency at once:

```bash
python3 -m pip install -r requirements.txt
```

## Install from a Pre-Downloaded Wheel

In environments without access to PyPI (for example, an air-gapped CI runner or a locked-down server), download the wheel for your platform and install it from a local file.

Download the wheel that matches your operating system and CPU architecture from the [GroupDocs.Metadata releases](https://releases.groupdocs.com/metadata/python-net/) page:

| Platform | Wheel file name ends with |
| --- | --- |
| Windows 64-bit | `py3-none-win_amd64.whl` |
| Linux x64 (glibc) | `py3-none-manylinux1_x86_64.whl` |
| macOS Apple Silicon (M-series) | `py3-none-macosx_11_0_arm64.whl` |
| macOS Intel | `py3-none-macosx_10_14_x86_64.whl` |

Install the downloaded file with `pip` (replace the file name with the wheel you downloaded):

{{< tabs "install-from-wheel">}}
{{< tab "Windows" >}}
```ps
py -m pip install .\groupdocs_metadata_net-26.5-py3-none-win_amd64.whl
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 -m pip install ./groupdocs_metadata_net-26.5-py3-none-manylinux1_x86_64.whl
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 -m pip install ./groupdocs_metadata_net-26.5-py3-none-macosx_11_0_arm64.whl
```
{{< /tab >}}
{{< /tabs >}}

## Verify the Installation

Confirm the package is installed and importable:

```bash
python3 -c "from groupdocs.metadata import Metadata; print('GroupDocs.Metadata is ready')"
```

You can also check the installed version with `pip`:

```bash
python3 -m pip show groupdocs-metadata-net
```

## Next Steps

- Follow the [Quick Start Guide]({{< ref "/metadata/python-net/getting-started/quick-start-guide.md" >}}) to read and remove metadata in a few minutes.
- Clone the [examples repository](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET) and read [How to Run Examples]({{< ref "/metadata/python-net/getting-started/how-to-run-examples.md" >}}).
- If you work with AI agents or LLMs, see [Agents and LLM Integration]({{< ref "/metadata/python-net/agents-and-llm-integration.md" >}}) for MCP and `AGENTS.md` details.
