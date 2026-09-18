---
id: mcp-troubleshooting-faq
url: metadata/mcp/troubleshooting-faq
title: Troubleshooting & FAQ
weight: 5
description: "Solutions to the most common GroupDocs.Metadata MCP server issues — server not appearing in the client, startup failures, missing native dependencies, and first-launch timeouts."
keywords: MCP server not showing up in Claude Desktop, Claude can't see MCP tools, MCP server failed to start, dnx command not found, libgdiplus not found error, read document metadata AI, remove metadata before sharing, EXIF GPS MCP, edit document properties agent
productName: GroupDocs.Metadata MCP Server
toc: True
---

Solutions to the most common GroupDocs.Metadata MCP server issues — server not appearing in the client, startup failures, missing native dependencies, and first-launch timeouts.

{{< alert style="info" >}}
**Platform-specific troubleshooting:** runtime problems depend on which build you run. For the `dnx` runner, native graphics libraries, and the Docker channel, see [Troubleshooting (.NET)]({{< ref "metadata/net/mcp/troubleshooting.md" >}}). The issues on this page apply to every platform.
{{< /alert >}}

## Why is my MCP server not showing up in Claude Desktop?

1. **Restart the client** — every client reads its MCP config only at startup.
2. Check the config file location for your OS ([per-client reference]({{< ref "metadata/net/mcp/install-in-ai-clients.md" >}})) and that the entry sits under the right root key (`mcpServers` for Claude Desktop/Cursor/Windsurf, `servers` for VS Code/VS 2022).
3. Validate the JSON — a trailing comma silently breaks the whole file. If you used the [installer](https://github.com/groupdocs/GroupDocs.Mcp.Installer), a timestamped `.bak` of your previous config sits next to the file for comparison.

## The first tool call is slow or fails once, then works

A **cold cache**: on the very first use the server's package or image is still downloading while the client is already waiting on the connection. Warming it once fixes it for good — the exact command depends on your build: [.NET]({{< ref "metadata/net/mcp/troubleshooting.md" >}}#the-first-tool-call-is-slow-or-fails-once-and-then-works).

## The server fails to start, or a runtime dependency is missing

These are properties of the build you run rather than of MCP, so the fixes live with the platform:

| Symptom | Where the fix is |
|---|---|
| `dnx: command not found` | [.NET troubleshooting]({{< ref "metadata/net/mcp/troubleshooting.md" >}}#dnx-command-not-found) — `dnx` ships inside the .NET 10 SDK |
| `DllNotFoundException: libgdiplus` on Linux/macOS | [.NET troubleshooting]({{< ref "metadata/net/mcp/troubleshooting.md" >}}#dllnotfoundexception-libgdiplus) — install the native graphics libraries, or use the Docker image |
| "docker daemon not reachable" | [.NET troubleshooting]({{< ref "metadata/net/mcp/troubleshooting.md" >}}#docker-daemon-not-reachable) — start Docker Desktop or `dockerd` |

## The agent says a file does not exist

Pass the **file name**, not a full path from your machine: the server resolves names inside its configured storage folder. When a name is not found the tool responds with the list of files it can see, so the agent can correct itself — check that list against [`GROUPDOCS_MCP_STORAGE_PATH`]({{< ref "metadata/net/mcp/configuration.md" >}}).

## Why does the agent only see five properties?

Evaluation mode. Without a license the engine returns **only the first five document properties**, plus reduced XMP and EXIF access. A metadata audit run unlicensed will look complete and be wrong. Check with [`get_license_status`]({{< ref "metadata/mcp/tools-reference/get-license-status.md" >}}) before trusting any result.

## Does removing metadata really remove it?

[`remove_metadata`]({{< ref "metadata/mcp/tools-reference/remove-metadata.md" >}}) strips the removable metadata the engine knows about and writes a **new cleaned file**. Two caveats worth stating plainly: some formats keep information that is structurally part of the document (tracked changes, embedded objects, previous revisions), and the *original* file still exists with everything in it. Share the cleaned copy, not the original.

## What is the difference between get_document_info and read_metadata?

[`get_document_info`]({{< ref "metadata/mcp/tools-reference/get-document-info.md" >}}) is the cheap structural check — type, size, pages, whether the file is encrypted. [`read_metadata`]({{< ref "metadata/mcp/tools-reference/read-metadata.md" >}}) enumerates the properties themselves. Ask the first when routing files, the second when the content of the properties matters.

## Can it read EXIF from photos, including GPS?

Yes, images are first-class here — EXIF, XMP, and IPTC. But GPS data and thumbnails are **unavailable in evaluation mode**, so a licensed run is required before concluding that a photo carries no location.

## Can it edit several properties at once?

[`write_metadata`]({{< ref "metadata/mcp/tools-reference/write-metadata.md" >}}) sets **one** property per call. An agent asked to change three will make three calls, each writing a file — make sure it chains them on the previous result rather than on the original, or only the last change survives.

## Verifying an installation end-to-end

Ask your agent *"list your GroupDocs metadata tools and the license status"* — it should name `read_metadata`, `search_metadata`, `write_metadata`, `remove_metadata`, `get_document_info`, `get_license_status`. For a scripted check that performs the real MCP handshake and a live call through the engine, see [verifying a .NET installation]({{< ref "metadata/net/mcp/troubleshooting.md" >}}#verifying-an-installation-end-to-end).

## Still stuck?

Post your config (redact license paths) and the client name in the [Metadata forum](https://forum.groupdocs.com/c/metadata/9) — we answer MCP questions daily. Bugs: [GitHub issues](https://github.com/groupdocs-metadata/GroupDocs.Metadata.Mcp/issues).
