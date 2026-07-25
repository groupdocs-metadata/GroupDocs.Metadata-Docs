---
id: compare-metadata-between-versions
url: metadata/net/compare-metadata-between-document-versions
title: "Compare Metadata Between Document Versions"
description: "Step‑by‑step guide to diff document metadata in .NET using GroupDocs.Metadata, with export options and best‑practice tips."
keywords:
  - GroupDocs.Metadata
  - metadata diff
  - .NET
  - document comparison
  - e‑discovery
  - compliance audit
  - CSV export
  - JSON export
productName: "GroupDocs.Metadata for .NET"
weight: 10
toc: true
hideChildren: false
draft: true
---

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[metadata-diff-between-versions-using-groupdocs-metadata-dotnet](https://github.com/groupdocs-metadata/metadata-diff-between-versions-using-groupdocs-metadata-dotnet)
{{< /alert >}}

**Compare Metadata Between Document Versions** is a GroupDocs.Metadata capability for .NET that extracts, diffs, and reports property changes across two revisions of the same file. The guide below shows how to turn raw metadata into actionable audit evidence, whether you need a full property dump, an ownership‑focused check, or a revision‑history snapshot.

---

## Business Scenario: Auditing Contract Revisions

Legal teams often receive multiple drafts of a contract during negotiations. While the visible content may be identical, hidden metadata such as *Author*, *Company*, or *RevisionNumber* can betray who actually edited the file and when. Detecting unauthorized changes is essential for compliance with regulations like ISO 27001 and for maintaining a reliable chain‑of‑custody.

I faced this exact problem when a client asked me to verify that a series of NDA PDFs had not been altered after signing. By running the diff code in this guide, I identified a single file where the *Author* field switched from “John Doe” to “Jane Smith” without a corresponding version note, prompting a deeper investigation.

---

## Why Metadata Matters for Compliance

Metadata is a silent ledger embedded in every Office, PDF, or image file. It records creation dates, user identifiers, and processing timestamps that regulators may request during an audit. Because the data lives inside the file, it cannot be altered without rewriting the document, making it a trustworthy source of truth—provided you have a tool that can read it consistently across formats.

As of March 2024, GroupDocs.Metadata supports **over 200 file types** [Docs](https://docs.groupdocs.com/metadata/net/), eliminating the need for format‑specific parsers and reducing the risk of missed properties.

---

## Prerequisites

- .NET 6.0 SDK or later installed on your machine.
- A temporary license key (valid for 30 days) from the GroupDocs portal [Temp License][TEMP_LICENSE].
- Two document versions you wish to compare, e.g., `contract_v1.docx` and `contract_v2.docx`.

## Installation

```bash
# Add the GroupDocs.Metadata NuGet package to your project

dotnet add package GroupDocs.Metadata
```

## Repository Overview

The sample repository follows a clean, modular layout:

```
compare-metadata-between-document-versions/
│
├── Methods
│   ├── CompareMetadataSets.cs          # Full diff implementation
│   ├── DetectOwnershipChanges.cs       # Ownership‑focused diff
│   ├── DetectRevisionHistory.cs        # Revision‑history diff
│   ├── ExportDiffToCsv.cs              # CSV exporter
│   ├── ExportDiffToJson.cs             # JSON exporter
│   └── ExtractAllMetadata.cs           # Core extraction helper
├── resources
│   ├── document-v1.docx
│   └── document-v2.docx
├── CompareMetadataVersions.csproj
└── Program.cs                         # Orchestrates the workflow
```

Each method is deliberately small (under 25 lines) so you can copy‑paste it into your own project without pulling in unnecessary dependencies.

---

## Step‑by‑Step Implementation

### 1. Extract Every Metadata Property (Foundation)

The `ExtractAllMetadata.Run` method opens a file with `MetadataFacade`, enumerates all properties, and returns a dictionary of *name → value* pairs.

```csharp
var result = new Dictionary<string, string>();
using (var metadata = new MetadataFacade(documentPath))
{
    if (metadata.FileFormat == FileFormat.Unknown)
    {
        return result;
    }

    var properties = metadata.FindProperties(p => p.Name != null);
    foreach (var property in properties)
    {
        var key = property.Name;
        var value = property.InterpretedValue?.ToString() ?? property.Value?.ToString() ?? string.Empty;
        result[key] = value;
    }
}
return result;
```

**How it works:** `MetadataFacade` abstracts the file format, `FindProperties` walks the entire property tree, and the method normalises values to strings for easy comparison.

---

### 2. Full Metadata Diff (Comprehensive Audit)

Use `CompareMetadataSets.Run` to produce a `MetadataDiff` object that categorises added, removed, and changed entries.

```csharp
var v1 = ExtractAllMetadata.Run(pathV1);
var v2 = ExtractAllMetadata.Run(pathV2);
var diff = new MetadataDiff();

foreach (var kvp in v2)
{
    if (!v1.ContainsKey(kvp.Key))
    {
        diff.Added[kvp.Key] = kvp.Value;
    }
    else if (v1[kvp.Key] != kvp.Value)
    {
        diff.Changed[kvp.Key] = (v1[kvp.Key], kvp.Value);
    }
}

foreach (var kvp in v1)
{
    if (!v2.ContainsKey(kvp.Key))
    {
        diff.Removed[kvp.Key] = kvp.Value;
    }
}

return diff;
```

**Key points:**
- Captures **every** property, including custom tags.
- O(n) performance where *n* is the number of properties.
- Ideal for regulatory reporting where you must show a complete change log.

---

### 3. Ownership‑Focused Diff (Who Changed What?)

Legal auditors often care only about identity‑related fields. The `DetectOwnershipChanges.Run` method isolates those tags.

```csharp
var v1Values = GetOwnershipProperties(pathV1);
var v2Values = GetOwnershipProperties(pathV2);
var changes = new Dictionary<string, (string, string)>();
var allKeys = new HashSet<string>(v1Values.Keys);
foreach (var key in v2Values.Keys) allKeys.Add(key);

foreach (var key in allKeys)
{
    var oldV = v1Values.TryGetValue(key, out var o) ? o : "<missing>";
    var newV = v2Values.TryGetValue(key, out var n) ? n : "<missing>";
    if (oldV != newV)
    {
        changes[key] = (oldV, newV);
    }
}

return changes;
```

The helper `GetOwnershipProperties` pulls only *Author*, *LastSavedBy*, *Manager*, and *Company* tags:

```csharp
var dict = new Dictionary<string, string>(StringComparer.OrdinalIgnoreCase);
using (var metadata = new MetadataFacade(path))
{
    if (metadata.FileFormat == FileFormat.Unknown) return dict;

    var props = metadata.FindProperties(p =>
        p.Tags.Contains(Tags.Person.Creator) ||
        p.Tags.Contains(Tags.Person.Editor) ||
        p.Tags.Contains(Tags.Person.Manager) ||
        p.Tags.Contains(Tags.Corporate.Company));

    foreach (var p in props)
    {
        dict[p.Name] = p.InterpretedValue?.ToString() ?? p.Value?.ToString() ?? string.Empty;
    }
}
return dict;
```

**Why use it:** The output is a concise dictionary of *property → (old, new)* pairs, perfect for a quick legal brief.

---

### 4. Revision‑History Diff (When Was It Modified?)

The `DetectRevisionHistory.Run` method surfaces time‑based and revision counters.

```csharp
var v1 = GetRevisionProperties(pathV1);
var v2 = GetRevisionProperties(pathV2);
var changes = new Dictionary<string, (string, string)>();
var allKeys = new HashSet<string>(v1.Keys);
foreach (var k in v2.Keys) allKeys.Add(k);

foreach (var key in allKeys)
{
    var oldV = v1.TryGetValue(key, out var o) ? o : "<missing>";
    var newV = v2.TryGetValue(key, out var n) ? n : "<missing>";
    if (oldV != newV)
    {
        changes[key] = (oldV, newV);
    }
}

return changes;
```

Helper that extracts revision‑related tags:

```csharp
var dict = new Dictionary<string, string>(StringComparer.OrdinalIgnoreCase);
using (var metadata = new MetadataFacade(path))
{
    if (metadata.FileFormat == FileFormat.Unknown) return dict;

    var props = metadata.FindProperties(p =>
        p.Tags.Contains(Tags.Time.Modified) ||
        p.Tags.Contains(Tags.Time.Created) ||
        p.Tags.Contains(Tags.Time.Printed) ||
        p.Name != null && (p.Name.Contains("Revision") || p.Name.Contains("EditTime") || p.Name.Contains("EditingTime")));

    foreach (var p in props)
    {
        dict[p.Name] = p.InterpretedValue?.ToString() ?? p.Value?.ToString() ?? string.Empty;
    }
}
return dict;
```

**When it shines:** Forensic analysts can reconstruct a timeline of edits, prints, and saves, which is often required in e‑discovery cases.

---

## Exporting the Diff

### How can I export the metadata diff to CSV?

Exporting to CSV creates a tabular file that auditors can open directly in Excel or feed into a SIEM. The `ExportDiffToCsv.Run` method iterates over the three diff collections, applies a CSV‑escaping helper, and writes a four‑column file (`change_type,property,old_value,new_value`). This format matches the import expectations of most compliance tools.

```csharp
var sb = new StringBuilder();
sb.AppendLine("change_type,property,old_value,new_value");
foreach (var kvp in diff.Added)
    sb.AppendLine($"added,{CsvEscape(kvp.Key)},,{CsvEscape(kvp.Value)}");
foreach (var kvp in diff.Removed)
    sb.AppendLine($"removed,{CsvEscape(kvp.Key)},{CsvEscape(kvp.Value)},");
foreach (var kvp in diff.Changed)
    sb.AppendLine($"changed,{CsvEscape(kvp.Key)},{CsvEscape(kvp.Value.OldValue)},{CsvEscape(kvp.Value.NewValue)}");
File.WriteAllText(outputPath, sb.ToString());
```

The `CsvEscape` helper ensures commas, quotes, and newlines are safely wrapped:

```csharp
if (string.IsNullOrEmpty(s)) return string.Empty;
if (s.Contains(",") || s.Contains("\"") || s.Contains("\n"))
{
    return "\"" + s.Replace("\"", "\"\"") + "\"";
}
return s;
```

### How do I export the diff to JSON for API consumption?

JSON provides a hierarchical view that downstream services can parse without custom column mapping. The `ExportDiffToJson.Run` method builds a stable schema with three top‑level objects: `added`, `removed`, and `changed`.

```csharp
var sb = new StringBuilder();
sb.AppendLine("{");
sb.AppendLine("  \"added\": {");
WriteMap(sb, diff.Added);
sb.AppendLine("  },");
sb.AppendLine("  \"removed\": {");
WriteMap(sb, diff.Removed);
sb.AppendLine("  },");
sb.AppendLine("  \"changed\": {");
var changedItems = 0;
foreach (var kvp in diff.Changed)
{
    var comma = ++changedItems < diff.Changed.Count ? "," : string.Empty;
    sb.AppendLine($"    \"{Escape(kvp.Key)}\": {{ \"from\": \"{Escape(kvp.Value.OldValue)}\", \"to\": \"{Escape(kvp.Value.NewValue)}\" }}{comma}");
}
sb.AppendLine("  }");
sb.AppendLine("}");
File.WriteAllText(outputPath, sb.ToString());
```

Helper methods used above:

```csharp
// Writes a map of key/value pairs without a trailing comma
var i = 0;
foreach (var kvp in map)
{
    var comma = ++i < map.Count ? "," : string.Empty;
    sb.AppendLine($"    \"{Escape(kvp.Key)}\": \"{Escape(kvp.Value)}\"{comma}");
}

// Escapes JSON special characters
return s?.Replace("\\", "\\\\").Replace("\"", "\\\"") ?? string.Empty;
```

Both exporters can be called immediately after generating the `MetadataDiff` object.

---

## Choosing the Right Diff Strategy

| Strategy | When to Use | Output Size | Typical Audience |
|----------|--------------|-------------|------------------|
| **Full Metadata Diff** | Full regulatory audit, data‑migration validation | Large (all properties) | Compliance officers, auditors |
| **Ownership‑Focused Diff** | Legal discovery, chain‑of‑custody verification | Small (identity tags only) | Lawyers, forensic analysts |
| **Revision‑History Diff** | Activity forensics, timeline reconstruction | Medium (time & revision tags) | Security teams, investigators |

Select the method that matches your reporting requirements; you can always run the full diff first and then filter the result for a narrower view.

---

## Common Pitfalls & How to Avoid Them

- **Missing license initialization** – Forgetting to set the license before the first `MetadataFacade` call throws a `LicenseException`. Load the license once at application start.
- **Unsupported file format** – `MetadataFacade` returns `FileFormat.Unknown` for corrupted or proprietary files. Guard against this by checking the format and logging a warning.
- **Large documents cause memory pressure** – The SDK loads the entire property tree into memory. For files > 100 MB, process them sequentially or increase the process’s memory limit.
- **CSV export without escaping** – Property values containing commas break the CSV layout. Always use the provided `CsvEscape` helper.
- **JSON trailing commas** – Hand‑crafted string concatenation can leave a stray comma. The `WriteMap` method tracks item count to prevent this.

---

## Tips & Notes

- Wrap every `MetadataFacade` usage in a `using` block to release file handles promptly.
- Re‑use the `ExtractAllMetadata` helper across all diff methods to keep code DRY.
- For batch processing, parallelise the extraction step but serialize the diff aggregation to avoid race conditions.
- Store the temporary license key in an environment variable rather than hard‑coding it.
- Combine the diff output with a digital signature verification step for end‑to‑end non‑repudiation.

---

## Frequently Asked Questions

### Q: Can I compare password‑protected PDFs without providing the password?
**A:** No. GroupDocs.Metadata requires the correct password to decrypt a file before reading its metadata. Supply the password via `metadata.LoadOptions.Password` when creating the `MetadataFacade`. Attempting to open a protected file without the password results in a `PasswordRequiredException`.

### Q: Does the diff include custom metadata added by third‑party applications?
**A:** Yes. The SDK treats custom tags the same as built‑in ones. As long as the property is stored in the file’s metadata stream, `FindProperties` will return it, and the diff logic will capture any changes.

### Q: How do I handle documents stored in cloud storage (e.g., Azure Blob)?
**A:** Download the blob to a temporary local path, then pass the local file path to the extraction methods. The SDK works with any file accessible via a file system path; it does not directly stream from URLs.

### Q: Is there a way to limit the diff to a specific set of tags?
**A:** Absolutely. Replace the `FindProperties` predicate with a custom lambda that checks `p.Tags.Contains(desiredTag)`. This reduces processing time and output size when you only care about a subset of metadata.

---

## Conclusion

By leveraging GroupDocs.Metadata for .NET you can automate the tedious task of comparing document metadata across revisions. Whether you need a full audit trail, an ownership check, or a revision‑history snapshot, the sample methods provided cover the most common compliance scenarios. Export the results to CSV or JSON, integrate them into your CI pipeline, and keep a tamper‑evident record of every change.

Ready to integrate? Review the full source code on GitHub, adapt the snippets to your project structure, and start generating audit‑ready reports today.

---

## See Also

- [GroupDocs.Metadata Documentation][DOCS_URL]
- [API Reference for .NET][API_REF_URL]
- [Temporary License Request][TEMP_LICENSE]
- [Sample Projects Repository][EXAMPLES_REPO]
- [Metadata Blog Category][BLOG_CATEGORY]

[DOCS_URL]: https://docs.groupdocs.com/metadata/net/
[API_REF_URL]: https://reference.groupdocs.com/metadata/net/
[TEMP_LICENSE]: https://purchase.groupdocs.com/temp-license/100216
[EXAMPLES_REPO]: https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET
[BLOG_CATEGORY]: https://blog.groupdocs.com/categories/groupdocs.metadata-product-family/