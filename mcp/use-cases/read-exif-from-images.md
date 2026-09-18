---
id: mcp-uc-read-exif-from-images
url: metadata/mcp/use-cases/read-exif-from-images
title: How to read EXIF, XMP, and IPTC data from images with AI
linkTitle: Read EXIF from images
weight: 4
description: "Read EXIF, XMP, and IPTC metadata from images with an AI agent over MCP: camera details, timestamps, and GPS coordinates, all locally."
keywords: read EXIF with AI agent, extract GPS from photo MCP, XMP IPTC reader agent, image metadata extraction local
productName: GroupDocs.Metadata MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "How to read EXIF, XMP, and IPTC data from images with AI"
        description: "Read EXIF, XMP, and IPTC metadata from images with an AI agent over MCP: camera details, timestamps, and GPS coordinates, all locally."
        steps:
        - name: "Install the server"
          text: "Run the GroupDocs.Metadata MCP server with Docker or dnx and register it in your AI client."
        - name: "Put the documents in the storage folder"
          text: "Point GROUPDOCS_MCP_STORAGE_PATH at the folder that holds the files the agent should use."
        - name: "Ask the agent"
          text: "What EXIF data do these photos have? Include camera, date taken, and GPS if present."
---

Images carry the richest metadata of anything on a normal disk: the camera and its settings, the moment of capture, the editing history, and often the exact location. All of it is one tool call away — and none of it leaves your machine.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server — the only platform available today. Installation and client setup: [MCP server for .NET]({{< ref "metadata/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## The prompt

> What EXIF data do these photos have? Include camera, date taken, and GPS if present.

[`read_metadata`]({{< ref "metadata/mcp/tools-reference/read-metadata.md" >}}) returns the packages the file carries — EXIF, XMP, IPTC — and the agent reads the answer out of them.

## Questions that work well

> Which of these photos were taken with the same camera?
> Sort these images by date taken, not by file date.
> Do any of these have GPS coordinates? Where?
> Does the XMP show which software edited this?

The second one is quietly useful: file timestamps get rewritten by copying and syncing; **EXIF date-taken does not**.

## The GPS caveat

**In evaluation mode GPS data and thumbnails are unavailable** — the read simply does not include them. An unlicensed check that reports "no location data" is not evidence of anything. Before concluding a photo is safe to publish:

> What is the license status of the metadata server?

[`get_license_status`]({{< ref "metadata/mcp/tools-reference/get-license-status.md" >}}); see [Licensing]({{< ref "metadata/mcp/getting-started/licensing.md" >}}).

## Cleaning images for publication

> Strip EXIF from every image in this folder and confirm the copies are clean.

[`remove_metadata`]({{< ref "metadata/mcp/tools-reference/remove-metadata.md" >}}) writes cleaned copies; a second read on the copy confirms. Remember the originals are still on disk with everything intact — publish the copies. Full workflow: [Strip metadata before sharing]({{< ref "metadata/mcp/use-cases/strip-metadata-before-sharing.md" >}}).

## Why local matters for photographs

Sending a photo to a cloud service to ask what it contains sends the photo — including the location you were trying to check for. Here the image is read in place by a local process, and only the answer travels; with a locally-hosted model, not even that. See [On-premise architecture]({{< ref "metadata/mcp/use-cases/on-premise-metadata-processing.md" >}}).
