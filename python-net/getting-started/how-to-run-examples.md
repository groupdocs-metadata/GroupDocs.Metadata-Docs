---
id: how-to-run-examples
url: metadata/python-net/how-to-run-examples
title: How to Run Examples
linkTitle: How to Run Examples
weight: 6
description: "Clone the GitHub examples repository, install dependencies into a virtual environment, optionally apply a license, and run every documented example end to end — locally, inside Docker, or on GitHub Actions."
keywords: run examples, examples repository, github, docker, dockerfile, CI, GitHub Actions, venv, virtual environment, run_all_examples, GROUPDOCS_LIC_PATH, GroupDocs.Metadata, python
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
toc: True
---

Every code example shown on this documentation site lives in a standalone, runnable form in the [GroupDocs.Metadata-for-Python-via-.NET](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET) repository on GitHub. Each example ships with its input sample files, so you can clone the repo and run anything with a single command.

## Prerequisites

Before running the examples, make sure you have:

1. **A supported platform and Python version** — see [System Requirements]({{< ref "/metadata/python-net/getting-started/system-requirements.md" >}}). Windows, Linux, and macOS (Intel and Apple Silicon) are all supported.
2. **Git** — or download the repository as a ZIP from GitHub.
3. **A license file** (optional but recommended) — without one, the library runs in evaluation mode (only the first few properties of each package are read and `save()` raises an "Evaluation only" exception). See [Licensing]({{< ref "/metadata/python-net/getting-started/evaluation-limitations-and-licensing.md" >}}) for how to obtain a free temporary license.

## Get the Code

Clone the repository and navigate into it:

```bash
git clone https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET.git
cd GroupDocs.Metadata-for-Python-via-.NET
```

## Project Structure

The repository mirrors this documentation tree. Every documentation page maps to a folder under `Examples/`, and every tabbed code block on a page maps to a `.py` file inside that folder. Input sample files live next to the script that reads them.

```md
📂 GroupDocs.Metadata-for-Python-via-.NET
├── README.md
├── LICENSE
├── AGENTS.md                          ← extracted from the pip package for AI tools
├── Dockerfile                         ← runs the whole suite on Linux
├── .github/workflows/run-examples.yml ← CI: runs all examples on every push
└── Examples
    ├── requirements.txt
    ├── run_all_examples.py
    ├── getting-started
    │   └── quick-start-guide
    │       ├── read_metadata.py
    │       ├── remove_metadata.py
    │       ├── document_info.py
    │       └── input.docx
    ├── developer-guide
    │   ├── basic-usage
    │   │   ├── get-document-info/
    │   │   ├── find-metadata-properties/
    │   │   ├── set-metadata-properties/
    │   │   ├── remove-metadata-properties/
    │   │   └── clean-metadata/
    │   └── advanced-usage
    │       ├── adding-metadata/
    │       ├── extracting-metadata/
    │       ├── removing-metadata/
    │       ├── getting-known-property-descriptors/
    │       ├── working-with-interpreted-values/
    │       ├── loading-files/                    (local disk, stream, specific format, password)
    │       ├── saving-files/                     (location, stream, original source)
    │       ├── working-with-metadata-properties/ (export, extract property values)
    │       └── working-with-metadata-standards/  (EXIF, IPTC IIM, XMP)
    └── licensing
        ├── set_license_from_file.py
        ├── set_license_from_stream.py
        └── set_metered_license.py
```

## Setup

1. **Create and activate a virtual environment**:

   Create:

   {{< tabs "venv-create">}}
   {{< tab "Windows" >}}
   ```ps
   py -m venv .venv
   ```
   {{< /tab >}}
   {{< tab "Linux" >}}
   ```bash
   python3 -m venv .venv
   ```
   {{< /tab >}}
   {{< tab "macOS" >}}
   ```bash
   python3 -m venv .venv
   ```
   {{< /tab >}}
   {{< /tabs >}}

   Activate:

   {{< tabs "venv-activate">}}
   {{< tab "Windows" >}}
   ```ps
   .venv\Scripts\activate
   ```
   {{< /tab >}}
   {{< tab "Linux" >}}
   ```bash
   source .venv/bin/activate
   ```
   {{< /tab >}}
   {{< tab "macOS" >}}
   ```bash
   source .venv/bin/activate
   ```
   {{< /tab >}}
   {{< /tabs >}}

2. **Install dependencies** from `Examples/requirements.txt`:

   {{< tabs "install-deps">}}
   {{< tab "Windows" >}}
   ```ps
   py -m pip install -r Examples/requirements.txt
   ```
   {{< /tab >}}
   {{< tab "Linux" >}}
   ```bash
   python3 -m pip install -r Examples/requirements.txt
   ```
   {{< /tab >}}
   {{< tab "macOS" >}}
   ```bash
   python3 -m pip install -r Examples/requirements.txt
   ```
   {{< /tab >}}
   {{< /tabs >}}

3. **Configure a license** (optional). The suite honours the `GROUPDOCS_LIC_PATH` environment variable and also picks up any `*.lic` file dropped into the project root. Set the variable in your shell before running `run_all_examples.py`:

   {{< tabs "license-env">}}
   {{< tab "Windows (PowerShell)" >}}
   ```ps
   $env:GROUPDOCS_LIC_PATH = "C:\path\to\GroupDocs.Metadata.lic"
   ```
   {{< /tab >}}
   {{< tab "Linux" >}}
   ```bash
   export GROUPDOCS_LIC_PATH="/path/to/GroupDocs.Metadata.lic"
   ```
   {{< /tab >}}
   {{< tab "macOS" >}}
   ```bash
   export GROUPDOCS_LIC_PATH="/path/to/GroupDocs.Metadata.lic"
   ```
   {{< /tab >}}
   {{< /tabs >}}

   {{< alert style="info" >}}
   Learn more about licensing, evaluation limits, and how to obtain a free 30-day temporary license in the [Licensing]({{< ref "/metadata/python-net/getting-started/evaluation-limitations-and-licensing.md" >}}) documentation topic.
   {{< /alert >}}

## Run the Examples

### Run the Full Suite

From the repository root, invoke `run_all_examples.py`. It runs every example in order, prints a per-file status line, and exits with a pass/fail summary.

{{< tabs "run-all">}}
{{< tab "Windows" >}}
```ps
py Examples\run_all_examples.py
```
{{< /tab >}}
{{< tab "Linux" >}}
```bash
python3 Examples/run_all_examples.py
```
{{< /tab >}}
{{< tab "macOS" >}}
```bash
python3 Examples/run_all_examples.py
```
{{< /tab >}}
{{< /tabs >}}

### Run a Single Example

Change into the folder that contains the script and run it directly. Input sample files live next to each script, so relative paths resolve correctly.

```bash
cd Examples/developer-guide/basic-usage/get-document-info
python get_document_info.py
```

Examples that modify a file write their output artefacts into the same folder as the script. When an example is documented on this site, the generated artefact is also linked from an **output** tab next to the code block — click it to download the file that a successful run produces.

## Run with Docker

The repository includes a `Dockerfile` (based on `python:3.13-slim`) that installs the required native libraries (`libgdiplus`, `libfontconfig1`, ICU) and every Python dependency, then runs the full suite. Use it when you want a clean, reproducible Linux environment without touching your host machine:

```bash
docker build -t groupdocs-metadata-examples .
docker run --rm \
    -e GROUPDOCS_LIC_PATH=/license/GroupDocs.Metadata.lic \
    -v /path/to/your/license:/license:ro \
    groupdocs-metadata-examples
```

Drop the `-e` / `-v` flags to run in evaluation mode.

## Continuous Integration

Every push triggers `.github/workflows/run-examples.yml`, which runs the entire example suite on `ubuntu-latest` with Python 3.13 (after installing `libicu-dev`, `libgdiplus`, and `libfontconfig1`). Fork the repository and open a pull request — the workflow runs for free on GitHub-hosted runners and is a quick way to sanity-check local changes in a clean environment.

## Troubleshooting

- **`DllNotFoundException: libgdiplus`** on Linux / macOS — install the native dependencies. See [How to install libgdiplus]({{< ref "/metadata/python-net/getting-started/troubleshooting/how-to-install-libgdiplus.md" >}}) and the full list in [System Requirements]({{< ref "/metadata/python-net/getting-started/system-requirements.md" >}}).
- **Garbled text or missing glyphs** — install Microsoft TrueType fonts (`ttf-mscorefonts-installer` on Debian / Ubuntu; macOS already ships them). Run `fc-cache -f` after installing so fontconfig picks them up.
- **"Evaluation only" exception on `save()`, or only a few properties are read** — you are running unlicensed. Set `GROUPDOCS_LIC_PATH` to a valid license file and re-run. See [Licensing]({{< ref "/metadata/python-net/getting-started/evaluation-limitations-and-licensing.md" >}}).
- **Anything else** — post on the [free support forum](https://forum.groupdocs.com/c/metadata) or visit the [Technical Support]({{< ref "metadata/python-net/technical-support" >}}) page.

## Contribute

If you would like to add or improve an example, we encourage you to contribute. All examples in this repository are open source and can be freely used in your own applications. Fork the repository, edit the example, and create a pull request — we will review the changes and include them if found helpful.
