---
id: mcp-uc-on-premise-metadata-processing
url: metadata/mcp/use-cases/on-premise-metadata-processing
title: "Running GroupDocs MCP servers on-premise: architecture and security model"
linkTitle: On-premise deployment
weight: 5
description: "Run metadata processing for AI agents fully on-premise: local stdio transport, no external endpoints, no inbound ports, no telemetry."
keywords: on-premise MCP server, air-gapped metadata processing, MCP security model, local document processing AI
productName: GroupDocs.Metadata MCP Server
toc: True
structuredData:
    showOrganization: True
    howTo:
        name: "Running GroupDocs MCP servers on-premise: architecture and security model"
        description: "Run metadata processing for AI agents fully on-premise: local stdio transport, no external endpoints, no inbound ports, no telemetry."
        steps:
        - name: "Run the pinned image inside the perimeter"
          text: "Start the GroupDocs.Metadata MCP server from its versioned Docker image as a child process of the AI client."
        - name: "Mount only the folders the agent may reach"
          text: "Map the document folder read-write and the license folder read-only."
        - name: "Choose the license mode"
          text: "Use a license file for fully offline operation; metered licensing needs outbound egress for usage reports."
---

Run metadata reading, editing, and removal for AI agents **fully on-premise**: the GroupDocs.Metadata MCP server uses local stdio transport with **no external endpoints, no inbound ports, and no telemetry**. This page is the one to send your security reviewer.

{{< alert style="info" >}}
The commands and config snippets on this page are for the **.NET** build of the server — the only platform available today. Installation and client setup: [MCP server for .NET]({{< ref "metadata/net/mcp/_index.md" >}}). Other platforms will expose the same tools with their own launch command; everything else on this page applies unchanged.
{{< /alert >}}

## The architecture in one picture

```text
+--------------+          +--------------------+         +------------------+
|  AI client   |  stdio   | MCP server process | reads / | local filesystem |
| (Claude, VS  | <----->  | (GroupDocs engine) | <-----> | storage / output |
| Code, agent) | JSON-RPC |   child process    |  writes |     folders      |
+--------------+          +--------------------+         +------------------+
```

* **Transport:** the AI client *starts the server as a child process* and communicates over standard input/output. The server never listens on a network socket.
* **Data path:** agent → local server → local filesystem. Files are read and written in the folders you configure; no file content is transmitted anywhere.
* **Network use:** only at install time (nuget.org or ghcr.io/docker.io). At runtime the server makes no outbound calls. Air-gapped: pre-pull the image or pre-cache the package and pin the version.
* **Telemetry:** none. The engine processes files in-process.

## Why this product in particular belongs on-premise

Metadata is where the sensitive residue lives: author names, machine and network paths, company fields inherited from templates, GPS coordinates in photographs. Sending a file to a cloud service **to find out whether it is safe to share** is self-defeating — the service now has the file and everything in it.

Here the file is read locally. What travels is the conversation: the properties the agent reports back to you. With a cloud-hosted model, "the author is Jane Smith and the photo was taken at these coordinates" reaches the model provider — which for a privacy review may be precisely the data you were protecting. Pair with a locally-hosted model and nothing leaves at all.

## Docker deployment inside the perimeter

```bash
docker run --rm -i \
  -v /srv/files:/data \
  -v /srv/licenses:/license:ro \
  -e GROUPDOCS_MCP_STORAGE_PATH=/data \
  -e GROUPDOCS_LICENSE_PATH=/license/GroupDocs.Metadata.lic \
  ghcr.io/groupdocs-metadata/metadata-net-mcp:26.9.0
```

* Pin the tag (`:26.9.0`, not `:latest`).
* Licence read-only; mount only the folders the agent should reach.
* Multi-arch images (linux/amd64 + linux/arm64) with all native dependencies included.

## License management

* **License file** — read from local disk by the local process. Fully offline; the right answer for air-gapped deployments, and the only way to get complete metadata reads.
* **Metered (pay-per-use)** — reports *usage* to GroupDocs servers, so it needs outbound egress. File content is never part of that report.

Both are covered in [Licensing]({{< ref "metadata/mcp/getting-started/licensing.md" >}}).

## What this fits — honestly

**A good fit:** pre-publication privacy checks, e-discovery preparation, bulk property correction after migrations, and any environment where files cannot be uploaded for inspection.

**Not what this is:** a DLP product or a scanning service. It answers questions about the files you point it at, one client on one machine — it does not watch a share, enforce a policy, or quarantine anything.

## FAQ

**Does any file content leave the machine?** Not from the server. Properties the agent reports back travel in the conversation to your model provider.

**Does it need internet at runtime?** No — only at install, and when metered licensing is enabled.

**Can I run it air-gapped?** Yes: pre-pull the image, use a license file, pin the version.

**What ports does it open?** None. stdio only.

**How do I prove that?** The [verification script]({{< ref "metadata/net/mcp/troubleshooting.md" >}}#verifying-an-installation-end-to-end) performs a real handshake and a real engine call, so you can watch exactly what happens.
