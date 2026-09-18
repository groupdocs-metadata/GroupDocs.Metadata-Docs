---
id: mcp-uc-strip-metadata-before-sharing
url: metadata/mcp/use-cases/strip-metadata-before-sharing
title: How to strip metadata from documents before sharing them
linkTitle: Strip metadata before sharing
weight: 2
description: "Remove metadata from documents and images with an AI agent over MCP before sharing them externally, and understand exactly what the cleaned copy still contains."
keywords: strip metadata before sharing, remove document properties AI, clean EXIF GPS agent, sanitize files before sending
productName: GroupDocs.Metadata MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to strip metadata from documents before sharing them"
        description: "Remove metadata from documents and images with an AI agent over MCP before sharing them externally, and understand exactly what the cleaned copy still contains."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Metadata MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "Strip all metadata from the files in my documents folder and tell me what you removed."
---

Sending a document outside the company sends its metadata too. This is the pass that prevents the familiar incident — a tender document arriving with a competitor's name in the Company field.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server — the only platform available today. Installation and client setup: [MCP server for .NET]({{< ref "metadata/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## The prompt

> Strip all metadata from the files in my documents folder and tell me what you removed.

[`remove_metadata`]({{< ref "metadata/mcp/tools-reference/remove-metadata.md" >}}) with no `categories` removes everything removable and writes a **cleaned copy** per file. Ask for the summary in the same breath so you can see what was there.

## Selective cleaning

Sometimes the dates or the title must survive:

> Remove the author and company fields but keep the title and dates.
> Strip only the EXIF from these photos — leave the document properties alone.

Pass the categories rather than clearing everything; the agent maps your words to them.

## Read first, then clean

A cleanup you cannot describe is not an audit trail. The stronger order:

1. [`read_metadata`]({{< ref "metadata/mcp/tools-reference/read-metadata.md" >}}) — capture what was there.
2. `remove_metadata` — produce the cleaned copy.
3. `read_metadata` on the **cleaned** file — confirm what remains.

Step 3 is the one people skip, and it is the one that turns "I think it is clean" into "here is the before and after".

## Three honest limits

* **The original still exists**, with everything in it. The tool writes a new file; sharing the wrong one defeats the exercise. Make the agent name the file it produced.
* **Not everything is metadata.** Tracked changes, comments, embedded objects, and earlier revisions live in the document body. For comments and annotations use the [GroupDocs.Annotation MCP server]({{< ref "annotation/mcp/_index.md" >}}); for content-level redaction, [GroupDocs.Redaction]({{< ref "redaction/mcp/_index.md" >}}).
* **Evaluation mode cannot be trusted for this.** With only the first five properties readable and writable, "everything removed" is not a claim the unlicensed engine can honestly make. [`get_license_status`]({{< ref "metadata/mcp/tools-reference/get-license-status.md" >}}) first.

## A repeatable habit

> Before I send anything from this folder: read the metadata, strip it, verify the copy is clean, and give me a one-line report per file.

That is four tool calls per file and a table at the end — the kind of routine an agent is genuinely good at, and a person reliably forgets.
