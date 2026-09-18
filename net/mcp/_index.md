---
id: mcp-net
url: metadata/net/mcp
title: MCP server for .NET
linkTitle: MCP Server
weight: 7
description: "Install and configure the GroupDocs.Metadata MCP server for .NET — one-click install links for VS Code and Cursor, per-OS setup for Windows, Linux, and macOS, and the full environment-variable reference."
keywords: GroupDocs.Metadata MCP .NET, install MCP server dnx, MCP server Docker image, MCP server configuration, Model Context Protocol .NET
productName: GroupDocs.Metadata MCP Server for .NET
hideChildren: True
toc: True
---

Everything needed to **install and run** the GroupDocs.Metadata MCP server on the .NET platform. What the server *does* — its tools, use cases, and licensing model — is platform-independent and lives in the [MCP server section]({{< ref "metadata/mcp/_index.md" >}}).

| The .NET build at a glance | |
|---|---|
| Package | [`GroupDocs.Metadata.Mcp`](https://www.nuget.org/packages/GroupDocs.Metadata.Mcp) (current **26.9.0**) |
| One-command run | `dnx GroupDocs.Metadata.Mcp --yes` |
| Container images | `ghcr.io/groupdocs-metadata/metadata-net-mcp` · `groupdocs/metadata-net-mcp` |
| Prerequisites | [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0) for the NuGet channel, or Docker |
| Source | [GroupDocs.Metadata.Mcp on GitHub](https://github.com/groupdocs-metadata/GroupDocs.Metadata.Mcp) |
| Release notes | [changelog](https://github.com/groupdocs-metadata/GroupDocs.Metadata.Mcp/tree/master/changelog) · [GitHub releases](https://github.com/groupdocs-metadata/GroupDocs.Metadata.Mcp/releases) |

## Start here

1. **Install** for your operating system — [Windows]({{< ref "metadata/net/mcp/windows-installation.md" >}}) · [Linux]({{< ref "metadata/net/mcp/linux-installation.md" >}}) · [macOS]({{< ref "metadata/net/mcp/macos-installation.md" >}})
2. **Register it in your AI client** — [one-click links and per-client configs]({{< ref "metadata/net/mcp/install-in-ai-clients.md" >}})
3. **Point it at your documents** — [configuration]({{< ref "metadata/net/mcp/configuration.md" >}})
4. **License it** — evaluation, a license file, or metered keys: [Licensing]({{< ref "metadata/mcp/getting-started/licensing.md" >}})

## In this section

* [Install on Windows]({{< ref "metadata/net/mcp/windows-installation.md" >}})
* [Install on Linux]({{< ref "metadata/net/mcp/linux-installation.md" >}})
* [Install on macOS]({{< ref "metadata/net/mcp/macos-installation.md" >}})
* [Register in AI clients]({{< ref "metadata/net/mcp/install-in-ai-clients.md" >}}) — VS Code, Cursor, Claude, Visual Studio, Windsurf, Cline, Codex, Rider
* [Configuration]({{< ref "metadata/net/mcp/configuration.md" >}}) — storage, output, license, metered keys
* [System requirements]({{< ref "metadata/net/mcp/system-requirements.md" >}})
* [Troubleshooting (.NET)]({{< ref "metadata/net/mcp/troubleshooting.md" >}}) — `dnx`, native libraries, Docker daemon

## Platform-independent reference

* [Tools reference]({{< ref "metadata/mcp/tools-reference/_index.md" >}}) — `read_metadata`, `search_metadata`, `write_metadata`, `remove_metadata`, `get_document_info`, `get_license_status`
* [Use cases]({{< ref "metadata/mcp/use-cases/_index.md" >}}) · [Supported formats]({{< ref "metadata/mcp/supported-formats.md" >}}) · [Troubleshooting & FAQ]({{< ref "metadata/mcp/troubleshooting-faq.md" >}})
