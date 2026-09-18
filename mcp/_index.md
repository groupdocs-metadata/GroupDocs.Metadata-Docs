---
id: mcp
url: metadata/mcp
title: GroupDocs.Metadata MCP Server
weight: 6
description: "GroupDocs.Metadata MCP server lets AI agents like Claude, Cursor, and Copilot read, edit, and strip document and image metadata — locally on your machine."
keywords: document metadata MCP server, read metadata with AI agent, strip metadata MCP, EXIF XMP MCP server, Claude document properties
productName: GroupDocs.Metadata MCP Server
hideChildren: True
toc: True
---

**GroupDocs.Metadata MCP server** lets AI agents like Claude, Cursor, and Copilot **read, edit, and strip metadata** from documents and images — PDF, Office, images, audio, video and more — **locally on your machine**. The files stay where they are; only the properties change. 

Run it with one command. The Docker image is self-contained — the runtime and every native dependency the engine needs are inside it:

```bash
docker run --rm -i -v $(pwd)/documents:/data \
  ghcr.io/groupdocs-metadata/metadata-net-mcp:latest
```

With the .NET 10 SDK installed, the same server also runs without Docker:

```bash
dnx GroupDocs.Metadata.Mcp --yes
```

Both are the **.NET** build of the server and run on Windows, Linux, and macOS. Other platforms will each get their own launcher — see [Install for your platform](#install-for-your-platform).

Or use the [guided installer]({{< ref "metadata/mcp/getting-started/_index.md" >}}) to register the server in your AI client, verify the setup, and configure shared folders in one pass.

## What you can do

Six tools (full details in the [tools reference]({{< ref "metadata/mcp/tools-reference/_index.md" >}})):

* **[`read_metadata`]({{< ref "metadata/mcp/tools-reference/read-metadata.md" >}})** — every property in the file as JSON: author, title, dates, custom fields, EXIF, XMP, IPTC.
* **[`search_metadata`]({{< ref "metadata/mcp/tools-reference/search-metadata.md" >}})** — only the properties matching a category, name, or value filter.
* **[`write_metadata`]({{< ref "metadata/mcp/tools-reference/write-metadata.md" >}})** — set, change, or add one property and save the updated file.
* **[`remove_metadata`]({{< ref "metadata/mcp/tools-reference/remove-metadata.md" >}})** — strip everything, or just the categories you name.
* **[`get_document_info`]({{< ref "metadata/mcp/tools-reference/get-document-info.md" >}})** — type, size, pages, encryption status, without enumerating properties.
* **[`get_license_status`]({{< ref "metadata/mcp/tools-reference/get-license-status.md" >}})** — which licensing mode is active and, under metered, how much has been consumed.

Ask in plain language — *"who wrote this?"*, *"strip everything before I send it"* — and the agent picks the tools.

## Install for your platform

Installation, prerequisites, and client configuration are platform-specific; the tools and licensing model below are the same everywhere.

| Platform | Status | Install and setup |
|---|---|---|
| .NET | **Available** | [MCP server for .NET]({{< ref "metadata/net/mcp/_index.md" >}}) |
| Java | Planned | [Tell us you need it](https://forum.groupdocs.com/c/metadata/9) |
| Python | Planned | [Tell us you need it](https://forum.groupdocs.com/c/metadata/9) |
| Node.js | Planned | [Tell us you need it](https://forum.groupdocs.com/c/metadata/9) |

{{< alert style="warning" >}}
**Read this before trusting a metadata audit.** In evaluation mode the engine returns **only the first five document properties**, with reduced XMP and EXIF access — and nothing in the response says so. An unlicensed privacy check can report a clean file that is not clean. Confirm with [`get_license_status`]({{< ref "metadata/mcp/tools-reference/get-license-status.md" >}}) first; see [Licensing]({{< ref "metadata/mcp/getting-started/licensing.md" >}}).
{{< /alert >}}

## Supported AI clients

| Client | How it connects |
|---|---|
| Claude Desktop | `claude_desktop_config.json` |
| Claude Code | `claude mcp add` CLI |
| VS Code / GitHub Copilot | user-level or workspace `mcp.json` |
| Visual Studio 2022 (17.14+) | `.mcp.json` in the solution root |
| Cursor | `~/.cursor/mcp.json` |
| Windsurf | `~/.codeium/windsurf/mcp_config.json` |
| Cline | Cline MCP settings |
| Codex CLI | `codex mcp add` CLI |
| JetBrains Rider | manual registration (Settings → AI Assistant → MCP) |

Exact config blocks for every client: [Register in AI clients]({{< ref "metadata/net/mcp/install-in-ai-clients.md" >}}).

## Delivery channels

| | Docker (recommended) | NuGet (`dnx`) |
|---|---|---|
| Prerequisites | Docker only | .NET 10 SDK (+ `libgdiplus` on Linux/macOS) |
| Native dependencies | bundled in the image | installed by you (or the setup script) |
| Package | `ghcr.io/groupdocs-metadata/metadata-net-mcp` | `GroupDocs.Metadata.Mcp` on NuGet |
| Architectures | linux/amd64 + linux/arm64 (Apple Silicon native) | any OS with .NET 10 |

## How it works

The server uses MCP's **local stdio transport**: your AI client starts the server as a child process and talks to it over standard input/output. No inbound ports, no external endpoints, no telemetry — the data path is *agent → local server → local filesystem*. Metadata is exactly the kind of data you do not want to hand to a cloud service to inspect: it is where author names, machine names, GPS coordinates, and internal paths hide. Details: [On-premise architecture]({{< ref "metadata/mcp/use-cases/on-premise-metadata-processing.md" >}}).

## When you need more than "File → Properties"

Every application shows a few properties for its own format. Choose this server when you need: **one model across documents, images, audio, and video**; the packages a UI hides (XMP, IPTC, EXIF including GPS); **value search** across every package at once; bulk edit and strip driven from a prompt rather than by hand; and the fidelity of the commercial GroupDocs engine trusted by enterprise teams for over a decade.

## Resources

* [Quick start]({{< ref "metadata/mcp/getting-started/_index.md" >}}) · [Use cases]({{< ref "metadata/mcp/use-cases/_index.md" >}}) · [Troubleshooting & FAQ]({{< ref "metadata/mcp/troubleshooting-faq.md" >}})
* GitHub: [server source](https://github.com/groupdocs-metadata/GroupDocs.Metadata.Mcp) · [installer](https://github.com/groupdocs/GroupDocs.Mcp.Installer) · [integration tests](https://github.com/groupdocs-metadata/GroupDocs.Metadata.Mcp.Tests)
* [NuGet package](https://www.nuget.org/packages/GroupDocs.Metadata.Mcp) · [Docker image](https://github.com/orgs/groupdocs-metadata/packages/container/package/metadata-net-mcp) · [MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=io.github.groupdocs-metadata/groupdocs-metadata-mcp)
* Questions: [Metadata forum](https://forum.groupdocs.com/c/metadata/9)
