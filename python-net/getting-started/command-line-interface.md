---
id: command-line-interface
url: metadata/python-net/command-line-interface
title: Command Line Interface
linkTitle: Command Line Interface
second_title: Inspect, clean and export metadata from the terminal with the groupdocs-metadata CLI
weight: 6
keywords: command line, CLI, terminal, groupdocs-metadata, metadata, sanitize, remove metadata, export metadata, EXIF, shell, pipeline, CI, GroupDocs.Metadata, python
description: "Inspect, clean and export document metadata straight from the terminal with the groupdocs-metadata command-line tool — no Python script required."
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
toc: True
---

Installing the `groupdocs-metadata-net` package also puts a `groupdocs-metadata` console script on your `PATH`. It is a thin wrapper over the Python API for the cases where a Python script is overkill — shell pipelines, Make rules, CI steps, and one-off checks before a file leaves your organization.

## Prerequisites

The CLI ships inside the package, so no extra installation is needed. Make sure `groupdocs-metadata-net` 26.9 or later is installed (see [Installation]({{< ref "/metadata/python-net/getting-started/installation.md" >}})), then check the console script:

```bash
groupdocs-metadata --version
```

```text
groupdocs-metadata 26.9.0
```

{{< alert style="info" >}}
If the `groupdocs-metadata` command is not found, the package's script directory is not on your `PATH`. The module form works everywhere and is equivalent: `python -m groupdocs.metadata`.
{{< /alert >}}

## Commands

Run `groupdocs-metadata --help` for the full flag listing, or `groupdocs-metadata <command> --help` for one command.

### info

Print basic information about a file.

```bash
groupdocs-metadata info photo.jpg
```

```text
format:     JPEG
extension:  .jpg
mime_type:  image/jpeg
size:       906062
pages:      1
encrypted:  False
```

Add `--json` for machine-readable output.

### show

List the metadata properties, one per line as `name = value  [tags]`. Where the engine can interpret a raw value, it is shown first with the raw value in parentheses.

```bash
groupdocs-metadata show report.docx --tag person
```

```text
Author = Prokofjev Igor  [person.creator, document.built_in]
LastSavedBy = New user  [person.editor, document.built_in]
dc:creator = Prokofjev Igor  [person.creator]
CommentAuthor = Prokofjev Igor  [person.creator]
CommentAuthorInitials = PI  [person.creator]
```

Without `--tag` every property is listed. `--json` prints a list of `{"name", "type", "value", "tags"}` objects (plus `"interpreted"` where there is one).

### clean

Remove every metadata property the engine detects.

```bash
groupdocs-metadata clean report.docx                   # writes report.clean.docx
groupdocs-metadata clean report.docx -o shared.docx    # choose the output
groupdocs-metadata clean report.docx -o report.docx    # clean in place
```

```text
removed 24 properties -> report.clean.docx
```

The input is never overwritten unless `-o` names it.

### remove

Remove only the properties that carry a tag.

```bash
groupdocs-metadata remove photo.jpg --tag person.creator -o photo-anon.jpg
groupdocs-metadata remove report.docx --tag person -o report-anon.docx
```

Some formats keep built-in properties such as a Word document's *Last saved by*: those are cleared rather than deleted, so `show` still lists them, empty.

### export

Export every property to a file. The output extension picks the format — `json`, `csv`, `xlsx`, `xls` or `xml`; pass `--format` when the name does not say.

```bash
groupdocs-metadata export report.pdf -o metadata.json
groupdocs-metadata export photo.jpg -o metadata.xlsx
```

### list-formats

List the file formats the engine supports — extension, format family and description.

```bash
groupdocs-metadata list-formats
```

## Tags

`show --tag` and `remove --tag` take either a whole category or one tag in it, written `category.tag`. `show` prints each property's tags in exactly that form, so its output can be pasted back into `--tag`.

| Category | Example tags |
| :- | :- |
| `person` | `person.creator`, `person.editor`, `person.manager`, `person.publisher`, `person.artist` |
| `time` | `time.created`, `time.modified`, `time.printed`, `time.total_editing_time` |
| `content` | `content.title`, `content.subject`, `content.keywords`, `content.comment`, `content.description` |
| `corporate` | `corporate.company`, `corporate.manager` |
| `document` | `document.built_in`, `document.read_only`, `document.hidden_data`, `document.user_comment` |
| `origin` | `origin.source`, `origin.template` |
| `legal` | `legal.copyright`, `legal.owner`, `legal.usage_terms` |
| `tool` | `tool.software`, `tool.hardware` |
| `property_type` | `property_type.hash`, `property_type.location`, `property_type.digital_signature` |

An unknown tag makes the command exit with code `2` and list the tags its category has.

## Global options

| Option | Description |
| :- | :- |
| `--license PATH` | Apply a license file before running the command. |
| `--version` | Print the CLI version and exit. |
| `--help` | Show usage help and exit. |

Every command that opens a file also takes `--password` for protected documents:

```bash
groupdocs-metadata --license GroupDocs.Metadata.lic show protected.docx --password "secret"
```

{{< alert style="info" >}}
The CLI also honours the `GROUPDOCS_LIC_PATH` environment variable — when it is set, the license is applied automatically and `--license` can be omitted. See [Evaluation Limitations and Licensing]({{< ref "/metadata/python-net/getting-started/evaluation-limitations-and-licensing.md" >}}).
{{< /alert >}}

Files are read through a stream, so a read-only input works for every command that does not overwrite it.

## Exit codes

| Code | Meaning |
| :- | :- |
| `0` | Success. |
| `2` | User error — missing input file, unknown tag or export format. |
| `1` | Engine error — the error message and its .NET exception type are printed on one line to standard error. |

## When to use the Python API instead

The CLI covers inspecting, cleaning and exporting single files. Setting or adding property values, working with a format's own packages (EXIF, XMP, IPTC objects), in-memory streams and export options need the Python API — see the [Developer Guide]({{< ref "/metadata/python-net/developer-guide" >}}).

## Next Steps

- [Quick Start Guide]({{< ref "/metadata/python-net/getting-started/quick-start-guide.md" >}}): read and remove metadata with the Python API.
- [Supported File Formats]({{< ref "/metadata/python-net/getting-started/supported-document-formats.md" >}}): the formats the engine reads and writes.
- [Troubleshooting]({{< ref "/metadata/python-net/getting-started/troubleshooting" >}}): common errors and their fixes.
