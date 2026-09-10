---
id: remove-pdf-metadata
url: /metadata/net/use-cases/remove-pdf-metadata/
title: 2 PDF Metadata Cleanup Methods for .NET - Sanitize vs Author Removal
weight: 10
description: "Compare Sanitize() and RemoveProperties to strip PDF Author, Creator, and XMP using GroupDocs.Metadata for .NET. Code, decision matrix, and verify-after-save guidance."
keywords: remove PDF metadata C#, metadata removal API, strip PDF author XMP, sanitize PDF .NET, GroupDocs.Metadata, document sanitization, comparison, performance, best practices, .NET PDF cleanup
productName: GroupDocs.Metadata for .NET
structuredData:
    showOrganization: True
toc: true
draft: true
---

{{< alert style="info" >}}
рџ’Ў Full working example available on GitHub:
[https://github.com/groupdocs-metadata/strip-pdf-metadata-dotnet](https://github.com/groupdocs-metadata/strip-pdf-metadata-dotnet)
{{< /alert >}}

## Introduction

PDF metadata cleanup is a GroupDocs.Metadata workflow for .NET that strips Author-style Info dictionary and XMP fields from PDFs before they leave a service. Author, Creator, Producer, Keywords, and XMP packets often survive "Save As" and browser downloads. Teams that share contracts need a server-side cleanup step that can prove Author-style identity is gone.

I reached for this pattern after a partner still saw `Author = Alice Example` on a draft we thought a browser cleaner had already scrubbed.

Two cleanup intensities show up in real pipelines: erase every detected package before a file leaves a trust boundary, or strip only person-tagged fields when Title and Subject must stay. GroupDocs.Metadata for .NET supports both on the same `Metadata` object.

This guide compares **2** primary cleanup methods for PDF documents using GroupDocs.Metadata for .NET, plus inspect and verify steps. Each method is evaluated on complexity, suitability, and maintenance.

## What This Guide Covers

You will see when to call `Sanitize()` for a full wipe, when to call `RemoveProperties` for Author-style fields only, how to baseline with `FindProperties`, and how to verify Author / `Person.Creator` after `Save`.

**Prerequisites:**
- .NET 8 SDK and NuGet access to GroupDocs.Metadata 26.8.0
- A PDF that still carries Info dictionary and/or XMP identity fields

{{< alert style="info" >}} 
**Complete Source Code:** All examples are available in our [GitHub repository](https://github.com/groupdocs-metadata/strip-pdf-metadata-dotnet). Clone, run, and customize for your needs.
{{< /alert >}}

## Quick Decision Matrix

| Scenario | Recommended Method | Why |
|----------|------------------|-----|
| File leaves the company / partner share | Sanitize all metadata | Clears Info and XMP packages in one call |
| Archive search still needs Title/Subject | Remove author metadata | Keeps descriptive fields, drops person identity |
| You do not know what the PDF carries | Inspect first | `FindProperties` prints the before baseline |
| Compliance asks "is Author gone?" | Verify after save | Re-open and check Author / Person.Creator |
| Browser cleaner already ran once | Prefer API Sanitize | GUI tools often miss XMP packets |

## Method Comparison Overview

| Method | Complexity | Performance | Flexibility | Best For |
|--------|-----------|-------------|-------------|----------|
| **Sanitize all metadata** | Low | Fast single pass | Low (all-or-nothing) | Outbound / untrusted exit |
| **Remove author metadata** | Medium | Fast filtered pass | High (predicate) | Keep Title/Subject, drop people |
| **Inspect metadata** | Low | Read-only | High (filters) | Before/after diagnostics |
| **Verify author removed** | Low | Read-only | Focused | Audit after Save |

## Detailed Method Analysis

### Method 1: Sanitize all PDF metadata

**Overview:** Calls `Sanitize()` on a loaded PDF to clear recognized metadata packages, including Info dictionary fields and XMP, then saves a new file. Use when no authorship trail should remain.

#### How It Works

Open the PDF with `Metadata`, call `Sanitize()`, print the removed-property count, and `Save` to an output path. The call targets detected packages rather than asking you to list every field name by hand. It is the blunt instrument for outbound hygiene.

#### When to Use

- вњ… Partner or public download links
- вњ… Cross-tenant document exchange
- вњ… "Leave no Author trail" policy exits

#### Pros and Cons

**Pros:**
- One call covers Info and XMP packages the API detects
- Small code surface; easy to wrap in a service method
- Returns a removal count you can log

**Cons:**
- Removes descriptive fields you may still need for filing
- After `Save`, Tool.Software Creator/Producer may be rewritten by the PDF engine
- Evaluation licensing can block unrestricted `Save`

#### Code Example

```csharp
using var metadata = new Metadata(inputPath);
int removed = metadata.Sanitize();
Console.WriteLine(removed);
metadata.Save(outputPath);
```

#### Performance Notes

For single-page contract PDFs the pass is dominated by open/save I/O. Log `removed` so operators can spot empty cleans versus large wipes.

---

### Method 2: Remove author-style PDF properties

**Overview:** Uses `RemoveProperties` with person/creator tags and Author/Creator/Producer name checks so identity is stripped while Title, Subject, and Keywords can remain.

#### How It Works

Build a predicate that matches `Tags.Person.Creator`, `Tags.Person.Editor`, and common identity property names. `RemoveProperties` deletes matches only. Save the result, then inspect again to confirm descriptive fields survived.

#### When to Use

- вњ… Records systems that index Title/Subject
- вњ… Redacting people from circulating drafts
- вњ… Policies that ban Author but allow keywords

#### Pros and Cons

**Pros:**
- Selective: keeps filing metadata that search depends on
- Predicate is explicit and reviewable in code review
- Same `Metadata` API as Sanitize

**Cons:**
- You must maintain the predicate as new identity fields appear
- Creator/Producer name matches can overlap tool fingerprints after Save

#### Code Example

```csharp
using var metadata = new Metadata(inputPath);
int removed = metadata.RemoveProperties(p =>
    p.Tags.Contains(Tags.Person.Creator) ||
    p.Tags.Contains(Tags.Person.Editor) ||
    string.Equals(p.Name, "Author", StringComparison.OrdinalIgnoreCase) ||
    string.Equals(p.Name, "Creator", StringComparison.OrdinalIgnoreCase) ||
    string.Equals(p.Name, "Producer", StringComparison.OrdinalIgnoreCase));
Console.WriteLine(removed);
metadata.Save(outputPath);
```

#### Performance Notes

Filtered removal still loads the metadata model once. Cost stays close to Sanitize for small PDFs; the win is semantic, not CPU.

---

### Method 3: Inspect PDF document metadata

**Overview:** Read-only listing of Author, Creator, Producer, Title, Subject, and Keywords for a before/after baseline.

#### When to Use

- вњ… First step in any cleanup job
- вњ… Debugging "we thought it was clean" tickets

#### Code Example

```csharp
using var metadata = new Metadata(inputPath);
var properties = metadata.FindProperties(p =>
    p.Tags.Contains(Tags.Person.Creator) ||
    p.Tags.Contains(Tags.Tool.Software) ||
    p.Tags.Contains(Tags.Content.Title) ||
    p.Tags.Contains(Tags.Content.Subject) ||
    string.Equals(p.Name, "Author", StringComparison.OrdinalIgnoreCase) ||
    string.Equals(p.Name, "Creator", StringComparison.OrdinalIgnoreCase) ||
    string.Equals(p.Name, "Producer", StringComparison.OrdinalIgnoreCase) ||
    string.Equals(p.Name, "Keywords", StringComparison.OrdinalIgnoreCase));

foreach (var property in properties)
{
    Console.WriteLine($"{property.Name} = {property.Value}");
}
```

---

### Method 4: Verify author metadata removed

**Overview:** Re-opens the cleaned PDF and prints `True` when Author / `Person.Creator` / `Person.Editor` leftovers are absent. Ignore Tool.Software Creator/Producer fingerprints rewritten on `Save`.

#### When to Use

- вњ… CI assertion after cleanup
- вњ… Distinguishing Author removal from engine stamps

#### Code Example

```csharp
using var metadata = new Metadata(inputPath);
var leftovers = metadata.FindProperties(p =>
    p.Tags.Contains(Tags.Person.Creator) ||
    p.Tags.Contains(Tags.Person.Editor) ||
    string.Equals(p.Name, "Author", StringComparison.OrdinalIgnoreCase));

Console.WriteLine(!leftovers.Any());
```

## Side-by-Side Feature Comparison

| Concern | Sanitize | Remove author properties |
|---|---|---|
| Clears XMP packages detected by API | Yes | Only when matched by predicate |
| Keeps Title / Subject / Keywords | No (typically cleared) | Yes (in this sample predicate) |
| Best policy fit | Full outbound wipe | Identity redaction |

## Real-World Use Cases

### Outbound contract pack

**Challenge:** Legal shares a draft PDF that still shows `Author = Alice Example` and Keywords marked confidential.

**Solution:** Run inspect, then `Sanitize()`, then verify Author is gone before uploading to the partner portal.

**Result:** Removal count is logged (for example 7), and verify prints `True` for Author-style leftovers.

### Internal archive with search fields

**Challenge:** Records still need Title and Subject, but person names must not travel in Author.

**Solution:** `RemovePdfAuthorMetadata` with the person/creator predicate, then inspect to confirm Title/Subject/Keywords remain.

**Result:** Author-style fields drop (for example 3 removals) while descriptive fields stay searchable.

## Why do Creator and Producer still appear after a successful clean?

Re-open the cleaned PDF and look only for Author / `Person.Creator` / `Person.Editor`. Residual Creator or Producer values tagged as Tool.Software are often PDF-engine fingerprints written on `Save`, not the original author identity. Treat those stamps as expected noise. Score the cleanup as failed only when Author-style leftovers remain. That distinction keeps CI checks honest and matches what compliance reviewers actually ask after a metadata wipe on GroupDocs.Metadata for .NET.

## FAQ

### Does Sanitize remove XMP as well as the Info dictionary?

`Sanitize()` clears recognized metadata packages detected on the PDF, including Info dictionary fields and XMP when present. Always inspect before and after on your own files.

### Can I keep Keywords while removing Author?

Yes. Use `RemoveProperties` with a person/creator-focused predicate like the sample.

### Which GroupDocs.Metadata version do these samples use?

GroupDocs.Metadata **26.8.0** on **.NET 8**.

## Conclusion

Pick **Sanitize** when a PDF exits a trust boundary and nothing identity-related should remain. Pick **RemoveProperties** when Title and Subject must survive. Wrap both with inspect and Author-focused verify so operators can see before/after evidence.

Next steps: clone [strip-pdf-metadata-dotnet](https://github.com/groupdocs-metadata/strip-pdf-metadata-dotnet), run the console app against `Resources/contract-with-metadata.pdf`, then wire the same methods into your upload service.

## See Also

- [GroupDocs.Metadata for .NET - remove all metadata](https://docs.groupdocs.com/metadata/net/removing-metadata/)
- [GroupDocs.Metadata for .NET - remove specific properties](https://docs.groupdocs.com/metadata/net/remove-metadata-properties/)
- [Blog article: remove PDF metadata (.NET)](https://blog.groupdocs.com/metadata/remove-pdf-metadata-net/)
- [GitHub sample repository](https://github.com/groupdocs-metadata/strip-pdf-metadata-dotnet)
- [API reference](https://reference.groupdocs.com/metadata/net/)

