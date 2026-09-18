---
id: mcp-uc-audit-document-metadata
url: metadata/mcp/use-cases/audit-document-metadata
title: How to audit document metadata with an AI agent
linkTitle: Audit document metadata
weight: 1
description: "Audit the metadata in your documents and images with an AI agent over MCP: read every property locally, search for specific values, and get a report of what the files reveal."
keywords: audit document metadata AI, what metadata does my file have, read document properties agent, metadata privacy check
productName: GroupDocs.Metadata MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to audit document metadata with an AI agent"
        description: "Audit the metadata in your documents and images with an AI agent over MCP: read every property locally, search for specific values, and get a report of what the files reveal."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Metadata MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "Does any property in these files still contain the old company name?"
---

Files carry more than their content: author names, machine names, company fields, editing history, camera data, GPS coordinates. Reading it is one tool call — and knowing what to ask is the whole skill.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server — the only platform available today. Installation and client setup: [MCP server for .NET]({{< ref "metadata/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## The pattern

1. Put the files in the storage folder the server can see.
2. Ask: *"What metadata does report.pdf carry?"*
3. The agent calls [`read_metadata`]({{< ref "metadata/mcp/tools-reference/read-metadata.md" >}}) and reports the properties grouped by package.
4. Follow up in plain language; the agent already holds the data.

## Ask precisely, get more

Broad reads produce long lists. Targeted questions produce answers:

> Does any property in these files still contain the old company name?
> Which of these photos have GPS coordinates?
> Who is listed as last-modified-by across the folder?
> Show me only the custom properties.

The second and third are [`search_metadata`]({{< ref "metadata/mcp/tools-reference/search-metadata.md" >}}) with a `valueContains` or `nameContains` filter — one call per file, and only the matches come back.

## The finding people are usually looking for

Three properties account for most privacy surprises:

* **Author / last-modified-by** — often a real person's name, sometimes a former employee's.
* **Company** — frequently a previous employer, inherited through a template.
* **GPS in EXIF** — a photo taken at home, shared publicly.

Ask for those three by name and you have covered the common cases in one pass.

## The licence caveat that matters here more than anywhere

Unlicensed, the engine returns **only the first five document properties**, and neither the response nor the agent mentions it. An audit that reports "nothing sensitive" may simply have stopped reading. Before you trust a result:

> What is the license status of the metadata server?

[`get_license_status`]({{< ref "metadata/mcp/tools-reference/get-license-status.md" >}}) answers in one call. Full detail: [Licensing]({{< ref "metadata/mcp/getting-started/licensing.md" >}}).

## Setup

```bash
dnx GroupDocs.Metadata.Mcp --yes
```

with `GROUPDOCS_MCP_STORAGE_PATH` pointing at the folder — [per-client config]({{< ref "metadata/net/mcp/install-in-ai-clients.md" >}}) or the [installer]({{< ref "metadata/mcp/getting-started/_index.md" >}}).

## Where to go next

* [Strip metadata before sharing]({{< ref "metadata/mcp/use-cases/strip-metadata-before-sharing.md" >}}) — the cleanup pass, and what it does not remove.
* [Correct document properties in bulk]({{< ref "metadata/mcp/use-cases/bulk-edit-document-properties.md" >}}) — one prompt, many files.
* [Read EXIF from images]({{< ref "metadata/mcp/use-cases/read-exif-from-images.md" >}}) — cameras, timestamps, and GPS.
* [On-premise architecture]({{< ref "metadata/mcp/use-cases/on-premise-metadata-processing.md" >}}) — why this one stays local.
