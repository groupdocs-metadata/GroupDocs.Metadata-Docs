---
id: agents-and-llm-integration
url: metadata/python-net/agents-and-llm-integration
title: Agents and LLM Integration
linkTitle: Agents and LLMs
description: "GroupDocs.Metadata for Python via .NET is AI agent and LLM friendly — machine-readable documentation, an MCP server, AGENTS.md shipped inside the pip package, and runnable code examples for metadata-driven AI pipelines."
weight: 9
keywords: AI, LLM, agent, RAG, MCP, machine-readable, documentation, Claude, GPT, Copilot, AGENTS.md, metadata
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
toc: True
---

## AI agent and LLM friendly

GroupDocs.Metadata for Python via .NET is designed to work seamlessly with AI agents, LLMs, and automated code generation tools. The library ships machine-readable documentation in multiple formats — including an `AGENTS.md` file inside the pip package itself — so AI assistants can discover and use the API without manual guidance.

## MCP server

GroupDocs provides an [MCP (Model Context Protocol) server](https://docs.groupdocs.com/mcp) that lets LLMs query the documentation on demand instead of loading it all at once. This saves tokens and lets your AI assistant fetch only what it needs for the current task.

To connect your AI tool to the MCP server, add the GroupDocs endpoint to your MCP configuration:

{{< tabs "mcp-setup">}}
{{< tab "Claude Code / Claude Desktop" >}}
```json
// Claude Code:    ~/.claude/settings.json (or project .mcp.json)
// Claude Desktop: ~/Library/Application Support/Claude/claude_desktop_config.json
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "GitHub Copilot" >}}
```json
// .vscode/mcp.json in your project root
{
  "servers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "Cursor" >}}
```json
// .cursor/mcp.json in your project root
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< tab "Generic MCP" >}}
```json
// Any MCP-compatible client
{
  "mcpServers": {
    "groupdocs-docs": {
      "url": "https://docs.groupdocs.com/mcp"
    }
  }
}
```
{{< /tab >}}
{{< /tabs >}}

See [https://docs.groupdocs.com/mcp](https://docs.groupdocs.com/mcp) for full setup instructions and the list of available tools.

## AGENTS.md — built into the package

The `groupdocs-metadata-net` pip package includes an `AGENTS.md` file at `groupdocs/metadata/AGENTS.md`. AI coding assistants that scan installed packages (such as Claude Code, Cursor, and GitHub Copilot) can automatically discover the API surface, usage patterns, and troubleshooting tips.

After installing the package, you can find it at:

```bash
pip show -f groupdocs-metadata-net | grep AGENTS
```

## Machine-readable documentation

Every documentation page is available as a plain Markdown file that AI tools can fetch and process directly:

| Resource | URL |
|---|---|
| Full documentation (single file) | `https://docs.groupdocs.com/metadata/python-net/llms-full.txt` |
| Full documentation (all products) | `https://docs.groupdocs.com/llms-full.txt` |
| Individual page (any page) | Append `.md` to the page URL |
| Quick start guide | `https://docs.groupdocs.com/metadata/python-net/getting-started/quick-start-guide.md` |

### How to use with AI tools

Point your AI assistant to the full documentation file for comprehensive context:

```
Fetch https://docs.groupdocs.com/metadata/python-net/llms-full.txt and use it
as a reference for GroupDocs.Metadata for Python via .NET API.
```

## Why GroupDocs.Metadata is a good building block for AI pipelines

Metadata is structured, high-signal context that LLMs and RAG systems can use directly — author, dates, comments, geolocation, camera settings, document statistics, and format details — without parsing the document body. GroupDocs.Metadata exposes this uniformly across 70+ formats:

- **Extract metadata for indexing / RAG** — pull author, title, keywords, and EXIF/XMP/IPTC fields into your search index or vector store.
- **Sanitize before sharing** — strip author, comments, and revision history from documents an agent is about to send out.
- **Compliance and DAM** — audit and normalize metadata across mixed document and image collections.
- **Export for analysis** — dump the metadata tree to CSV/XLSX/JSON for downstream tooling.

A typical extraction step looks like this:

```python
from groupdocs.metadata import Metadata

with Metadata("inbox/incoming.docx") as metadata:
    info = metadata.get_document_info()
    record = {
        "format": str(info.file_type.file_format),
        "pages": info.page_count,
        "encrypted": info.is_encrypted,
        "properties": {
            p.name: str(p.value)
            for p in metadata.find_properties(lambda p: True)
        },
    }
    # feed `record` into your index, vector store, or compliance pipeline
```

For end-to-end examples — reading and editing EXIF/XMP/IPTC, searching by tag, sanitizing, and exporting — see the [Developer Guide]({{< ref "metadata/python-net/developer-guide/_index.md" >}}). Every code block has a runnable counterpart in the [examples repository](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET).

## Pair with other GroupDocs products

For richer AI document pipelines, chain GroupDocs.Metadata with:

- [GroupDocs.Viewer for Python via .NET](https://docs.groupdocs.com/viewer/python-net/) — render documents to HTML/PNG/PDF for vision models and OCR.
- [GroupDocs.Conversion for Python via .NET](https://docs.groupdocs.com/conversion/python-net/) — normalize legacy and exotic formats before extraction.

## See also

- [Quick Start Guide]({{< ref "metadata/python-net/getting-started/quick-start-guide" >}}) — your first metadata script in five minutes
- [Developer Guide]({{< ref "metadata/python-net/developer-guide" >}}) — runnable examples for every API surface
- [API Reference](https://reference.groupdocs.com/metadata/python-net) — full class and method documentation
