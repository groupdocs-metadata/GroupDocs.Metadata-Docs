---
id: remove-pii-from-office-files
url: /metadata/net/use-cases/remove-pii-from-office-files/
title: How to Remove PII from Office Documents in .NET - 4 Practical Tutorials
weight: 1
description: "Learn four ways to remove personal data from Word, Excel, and PowerPoint metadata using GroupDocs.Metadata for .NET. Step-by-step tutorials with code, a selection cheat sheet, and a verification pass."
keywords: remove pii, document sanitization, office metadata, dotnet metadata api, metadata leak check, gdpr documents, clean docx properties, groupdocs metadata, tutorial, step by step
productName: GroupDocs.Metadata for .NET
structuredData:
    showOrganization: True
toc: true
draft: false
---

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[scrub-office-document-pii-dotnet](https://github.com/groupdocs-metadata/scrub-office-document-pii-dotnet)
{{< /alert >}}

## Getting Started

PII removal is a GroupDocs.Metadata capability for .NET that deletes identity-bearing properties from Office documents and then reads the file back to report what remains. These tutorials work through four approaches in the order a real project adopts them: clear the names, clear the field families around them, wipe everything at the boundary, and verify the result. Each one is a predicate over `MetadataProperty`, so the differences between them are small and the reasons to choose one over another are not.

The sample project behind this page seeds a DOCX with author, comment, revision, and SharePoint properties, then runs all four approaches plus a verification scan and asserts every outcome.

## What This Tutorial Covers

By the end you will know which property groups exist in an Office file, how to write a tag predicate and a name predicate, when `Sanitize()` is the better answer than four targeted passes, and how to prove a document is clean instead of assuming it.

## Prerequisites

- .NET SDK 8.0 or newer
- The `GroupDocs.Metadata` NuGet package, version 26.6.0 in the sample
- A DOCX, XLSX, or PPTX file that has been through a real review cycle; a freshly created file has almost nothing to remove

## Understanding the Problem

### Why Native Solutions Fall Short

The Windows properties dialog edits a handful of fields on one file at a time. It does not reach custom OOXML parts, it cannot be scripted across a folder, and it produces no record of what changed. Office itself has a Document Inspector, but it runs interactively and its results are not available to a build pipeline. Neither option answers the question an auditor asks six months later: which properties were removed from this file, and when.

### How GroupDocs.Metadata Solves This

One property search engine drives everything here. `RemoveProperties` accepts a lambda over `MetadataProperty` and returns the number of properties it deleted; `FindProperties` runs the same lambda in read-only mode. Properties also carry tags, so `Tags.Person.Creator` identifies author-style fields without hard-coding names that differ per format. That combination gives targeted cleanup, total cleanup, and verification from one API surface.

## Tutorial 1: Removing author and company fields - The Basics

### What You'll Learn

How to clear the four properties that name real people while leaving descriptive metadata usable.

### Step 1: Setup and Installation

Add the package to your project with `dotnet add package GroupDocs.Metadata --version 26.6.0`, then set a license path if you have one. Without a license the library runs in evaluation mode, which is fine for following along. The snippets below use the sample's alias `using MetadataFacade = GroupDocs.Metadata.Metadata;`, which avoids the namespace and class sharing a name; plain `Metadata` works just as well once the namespace is imported.

### Step 2: Basic Implementation

```csharp
using (var metadata = new MetadataFacade(inputPath))
{
    if (metadata.FileFormat == FileFormat.Unknown) return 0;
    var affected = metadata.RemoveProperties(p =>
        p.Tags.Contains(Tags.Person.Creator) ||
        p.Tags.Contains(Tags.Person.Editor) ||
        p.Tags.Contains(Tags.Person.Manager) ||
        p.Tags.Contains(Tags.Corporate.Company));
    metadata.Save(outputPath);
    return affected;
}
```

### Before and After

Before the pass, a reviewed board report typically carries `Author`, `LastSavedBy`, `Manager`, and `Company`. After it, those four are gone and `Title`, `Subject`, and `Keywords` are still there, which is what keeps a records system working. The returned count is the number to log.

### Common Issues and Solutions

If the count comes back zero on a file you know is dirty, check `FileFormat` first: the guard returns early on unrecognized input, and that looks identical to a clean file from the caller's side.

## Tutorial 2: Clearing field families - Enhanced Approach

### What You'll Learn

How to remove groups that tags do not classify, using name substrings instead.

### Step 1: Understanding the Enhanced Method

Comments, revisions, and server fields are families rather than single properties. `CommentsCount`, `Reviewer`, and `Reviewed` all belong to the review group; `Revision`, `TrackedChange`, `LastPrinted`, `TotalEditingTime`, and `EditTime` describe the editing timeline. Substring matching covers a family without listing every variant.

### Step 2: Implementation

```csharp
using (var metadata = new MetadataFacade(inputPath))
{
    if (metadata.FileFormat == FileFormat.Unknown) return 0;
    var affected = metadata.RemoveProperties(p =>
        p.Name != null && (
            p.Name.Contains("Revision") ||
            p.Name.Contains("TrackedChange") ||
            p.Name.Contains("LastPrinted") ||
            p.Name.Contains("TotalEditingTime") ||
            p.Name.Contains("EditTime")));
    metadata.Save(outputPath);
    return affected;
}
```

### Step 3: Advanced Configuration

The same shape covers SharePoint fields; only the substring list changes. Approver IDs, workflow paths, and content-type URIs describe internal structure, so this pass matters for anything exported from a document server:

```csharp
using (var metadata = new MetadataFacade(inputPath))
{
    if (metadata.FileFormat == FileFormat.Unknown) return 0;
    var affected = metadata.RemoveProperties(p =>
        p.Name != null && (
            p.Name.Contains("Server") ||
            p.Name.Contains("Workflow") ||
            p.Name.Contains("Approver") ||
            p.Name.Contains("ContentType") ||
            p.Name.Contains("Template")));
    metadata.Save(outputPath);
    return affected;
}
```

### Troubleshooting

Always null-check `p.Name` before calling `Contains`. Some packages expose unnamed entries, and the predicate runs against every property in the file, so one null reaches it eventually.

## Tutorial 3: The full sanitize - Advanced Techniques

### What You'll Learn

When one call replaces four predicates, and what you give up by using it.

### Step 1: Implementation

```csharp
using (var metadata = new MetadataFacade(inputPath))
{
    if (metadata.FileFormat == FileFormat.Unknown) return 0;
    var affected = metadata.Sanitize();
    metadata.Save(outputPath);
    return affected;
}
```

### Best Practices

`Sanitize()` clears every detected metadata package, including custom OOXML parts no targeted predicate looks for, which is why its count runs higher than the sum of the four passes. It also takes Title and Subject with it. Use it at the trust boundary and keep the targeted passes for documents that are still in use.

## Tutorial 4: Verifying the result - Enterprise Solution

### What You'll Learn

How to turn a cleanup into evidence by reading the saved file back.

### Step 1: Build the combined predicate

```csharp
var report = new LeakReport();
using (var metadata = new MetadataFacade(path))
{
    if (metadata.FileFormat == FileFormat.Unknown) return report;
    var props = metadata.FindProperties(p =>
        p.Tags.Contains(Tags.Person.Creator) ||
        p.Tags.Contains(Tags.Person.Editor) ||
        p.Tags.Contains(Tags.Person.Manager) ||
        p.Tags.Contains(Tags.Corporate.Company) ||
        (p.Name != null && (
            p.Name.Contains("Comment") ||
            p.Name.Contains("Reviewer") ||
            p.Name.Contains("Revision") ||
            p.Name.Contains("TrackedChange") ||
            p.Name.Contains("Classification") ||
            p.Name.Contains("Department") ||
            p.Name.Contains("Server") ||
            p.Name.Contains("Workflow"))));
```

### Step 2: Classify what came back

Empty values and zero counters are skipped, so a cleared counter does not read as a leak. Entries whose names begin with `Comment`, `Revision`, or `Inspection` are wrappers over body content and go to a separate list:

```csharp
    foreach (var p in props)
    {
        var value = p.InterpretedValue?.ToString() ?? p.Value?.ToString() ?? string.Empty;
        if (string.IsNullOrWhiteSpace(value)) continue;
        if (value == "0" || value == "0.0") continue;

        var entry = $"{p.Name}={value}";
        var name = p.Name ?? string.Empty;
        if (name.StartsWith("Comment") || name.StartsWith("Revision"))
        {
            report.ContentLevelLeaks.Add(entry);
        }
        else
        {
            report.MetadataLeaks.Add(entry);
        }
    }
}
return report;
```

### Security Considerations

`MetadataLeaks` must be empty before a file is treated as sanitized. `ContentLevelLeaks` stays informational: Word comments and tracked-change authors live inside `word/document.xml`, and a metadata library does not edit document body content. Removing those needs a content-editing library such as Aspose.Words, and that limitation belongs in your data-handling documentation rather than in a code comment.

## Quick Reference Guide

### Which method should I pick?

Pick by what has to survive. Identity pass when the file stays in circulation and Title and Subject are still needed. Field-family passes when review history or server fields are the sensitive part. `Sanitize()` when the file crosses the trust boundary and no metadata needs to live. Run the verification scan after any of them, every time.

| Situation | Method | Keeps descriptive fields |
|---|---|---|
| Draft shared with an external reviewer | Identity pass | Yes |
| Minutes published after an internal review | Comment pass | Yes |
| Filing where edit history is confidential | Revision pass | Yes |
| Export from a SharePoint library | Server pass | Yes |
| Publication to an open portal | `Sanitize()` | No |

## Frequently Asked Questions

**Q: Can I use multiple methods together?**
A: Yes, and in a batch job you should. The predicates are ordinary lambdas, so combining their conditions into one call means one file open and one save instead of four. Keep them separate only when the audit log needs a distinct count per group.

**Q: How do I handle errors in production?**
A: Guard on `FileFormat.Unknown` before every operation, keep the `using` block so handles close when a save throws, and treat a non-empty metadata-leak list as a failed job rather than a warning. The sample's `Program.cs` returns a non-zero exit code on any failed assertion.

**Q: What are the performance implications?**
A: The predicate evaluation is inexpensive; opening and rewriting the document dominates, so merging predicates matters more than optimizing the rules. I found that out the slow way after writing an exact-name list that missed `CommentsCount` and re-running the whole batch to catch it.

## Summary and Next Steps

Four approaches, one property engine, and a verification step that makes the outcome checkable. Start with the identity pass, add the field-family passes as your document sources demand them, reserve `Sanitize()` for the boundary, and run the leak check on every saved copy. Clone the repository and run it against one of your own reviewed documents to see which property groups your files actually carry.

## See Also

- [In-depth blog article about this project](https://blog.groupdocs.com/metadata/remove-pii-from-office-files-net/) – the business case and the full pipeline
- [Find metadata properties](https://docs.groupdocs.com/metadata/net/find-metadata-properties/) – reference for the search predicates behind the leak check
- [Remove metadata properties](https://docs.groupdocs.com/metadata/net/remove-metadata-properties/) – predicate-driven removal across formats
- [Product documentation](https://docs.groupdocs.com/metadata/net/) – getting started and advanced topics
- [API Reference](https://reference.groupdocs.com/metadata/net/) – full API details for GroupDocs.Metadata for .NET
