---
id: mcp-uc-bulk-edit-document-properties
url: metadata/mcp/use-cases/bulk-edit-document-properties
title: How to correct document properties in bulk with an AI agent
linkTitle: Correct properties in bulk
weight: 3
description: "Set and correct document metadata across many files with an AI agent over MCP, one property per call, chained so that every edit survives."
keywords: bulk edit document properties, set author metadata many files, fix document metadata AI, batch metadata update MCP
productName: GroupDocs.Metadata MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to correct document properties in bulk with an AI agent"
        description: "Set and correct document metadata across many files with an AI agent over MCP, one property per call, chained so that every edit survives."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Metadata MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "For every document in my folder, set Company to Acme Ltd and Author to Research Team, then tell me which files you changed."
---

Templates leave the wrong company in every file. A team rename leaves the old name in the author field. A migration leaves timestamps that make no sense. These are bulk metadata problems, and they are exactly what an agent with a loop is for.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server — the only platform available today. Installation and client setup: [MCP server for .NET]({{< ref "metadata/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## The prompt

> For every document in my folder, set Company to "Acme Ltd" and Author to "Research Team", then tell me which files you changed.

The agent calls [`write_metadata`]({{< ref "metadata/mcp/tools-reference/write-metadata.md" >}}) per file per property.

## The chaining rule

`write_metadata` sets **one property per call** and saves a **new file**. Two properties on one document means two calls, and the second must be applied to the **result** of the first. Otherwise the second call starts from the original and the first edit vanishes.

If an agent reports "both properties set" and only one stuck, this is why. Say it explicitly when it matters:

> Apply the second change to the file produced by the first, not to the original.

## Check before you write

A blind bulk write can overwrite good values with worse ones. One extra call per file makes it deliberate:

> First list the current Author and Company for each file. Then change only the ones that are wrong.

That is [`search_metadata`]({{< ref "metadata/mcp/tools-reference/search-metadata.md" >}}) with `nameContains`, and it turns a sweep into a decision.

## Format differences are real

Not every format has a place for every property. A write that a format cannot support **fails with a message** rather than pretending — good behaviour, but it means a mixed folder can produce a partial result. Ask for the failures as a list rather than a silent count.

## Keep it cheap

Under [metered licensing]({{< ref "metadata/mcp/getting-started/licensing.md" >}}#metered-pay-per-use-licensing) every call is billed usage, and a two-property sweep over 200 files is 400 calls plus the reads. Filter first, write second.

And the recurring caveat: in evaluation mode only the first five properties are visible, so a "check then write" workflow is checking a truncated list. [`get_license_status`]({{< ref "metadata/mcp/tools-reference/get-license-status.md" >}}) before a bulk run.
