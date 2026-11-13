---
id: remove-pdf-metadata
url: /metadata/java/use-cases/remove-pdf-metadata/
title: 2 PDF Metadata Cleanup Methods for Java - Sanitize vs Author Removal
weight: 10
description: "Compare sanitize() and removeProperties to strip PDF Author, Creator, and XMP using GroupDocs.Metadata for Java. Code, decision matrix, and verify-after-save guidance."
keywords: remove PDF metadata Java, metadata removal API, strip PDF author XMP, sanitize PDF Java, GroupDocs.Metadata, document sanitization, comparison, best practices, Java PDF cleanup
productName: GroupDocs.Metadata for Java
structuredData:
    showOrganization: True
toc: true
draft: false
---

## Introduction

PDF metadata cleanup is a GroupDocs.Metadata workflow for Java that removes identity fields from the Info dictionary and the XMP packet of a PDF before the file leaves your service. Author, Creator, Producer and Keywords are written by the tools that produce the document, and they survive "Save As", printing to PDF and most browser downloads. A server-side step is the only place where you can check every outgoing file the same way.

Real pipelines need one of two cleanup levels. Either everything goes, because the file crosses a trust boundary, or only the people go, because Title and Subject are still needed for search. GroupDocs.Metadata for Java supports both on the same `Metadata` object.

This guide compares **2** cleanup methods for PDF documents, plus an inspect step before the cleanup and a verify step after it.

## What This Guide Covers

You will see when to call `sanitize()` for a full wipe, when to call `removeProperties` for Author-style fields only, how to get a baseline with `findProperties`, and how to check Author and `Person.Creator` after `save`.

In Java, `findProperties` and `removeProperties` take a `Specification` object instead of a lambda. Specifications are combined with `.or(...)` and `.and(...)`, and the same instance can be reused for removal and for verification.

**Prerequisites:**
- Java 8 or later
- GroupDocs.Metadata for Java 26.11 from the [GroupDocs Maven repository]({{< ref "metadata/java/getting-started/installation.md" >}})
- A PDF that still carries Info dictionary or XMP identity fields

## Quick Decision Matrix

| Scenario | Recommended Method | Why |
|----------|------------------|-----|
| File leaves the company or goes to a partner share | Sanitize all metadata | Clears the Info dictionary and XMP in one call |
| Archive search still needs Title/Subject | Remove author metadata | Keeps descriptive fields, drops person identity |
| You do not know what the PDF carries | Inspect first | `findProperties` prints the baseline |
| Compliance asks "is Author gone?" | Verify after save | Re-open the file and check Author and `Person.Creator` |
| The PDF must keep its form fields or attachments | Remove author metadata | Since 26.11, `sanitize()` also removes them |

## Method Comparison Overview

| Method | Complexity | Flexibility | Best For |
|--------|-----------|-------------|----------|
| **Sanitize all metadata** | Low | Low (all-or-nothing) | Outbound files, untrusted recipients |
| **Remove author metadata** | Medium | High (specification) | Keep Title/Subject, drop people |
| **Inspect metadata** | Low | High (filters) | Before/after diagnostics |
| **Verify author removed** | Low | Focused | Audit after save |

## Detailed Method Analysis

### Method 1: Sanitize all PDF metadata

**Overview:** Calls `sanitize()` on a loaded PDF to clear the metadata packages GroupDocs.Metadata detects, including the Info dictionary and XMP, and saves a new file.

#### How It Works

Open the PDF with `Metadata`, call `sanitize()`, log the number of removed properties, and save the result to a new path. You don't list field names: the call works on whole packages.

Since version 26.11, `sanitize()` also removes annotations, attachments, bookmarks, form fields and digital signatures from PDF documents. Before 26.11 it removed annotations only.

#### When to Use

- Links for partners or public downloads
- Document exchange between tenants
- "No author trail" policies

#### Pros and Cons

**Pros:**
- One call covers the Info dictionary and XMP
- Small code that is easy to wrap in a service method
- Returns a count you can write to the log

**Cons:**
- Removes Title, Subject and other descriptive fields you may still need
- Removes attachments, bookmarks, form fields and signatures, which breaks fillable and signed PDFs
- After `save`, the PDF engine may write its own Producer value
- In evaluation mode the library does not save files, so you need a [license]({{< ref "metadata/java/getting-started/evaluation-limitations-and-licensing.md" >}})

#### Code Example

```java
try (Metadata metadata = new Metadata(inputPath)) {
    int removed = metadata.sanitize();
    System.out.println(removed);
    metadata.save(outputPath);
}
```

---

### Method 2: Remove author-style PDF properties

**Overview:** Uses `removeProperties` with person tags and Author/Creator/Producer name checks, so the identity is removed while Title, Subject and Keywords stay.

#### How It Works

Build a specification that matches `Tags.getPerson().getCreator()`, `Tags.getPerson().getEditor()` and the common identity property names. `WithNameSpecification` with `ignoreCase = true` matches a name regardless of case, so `Author` also finds the `author` property of the PDF document properties. `removeProperties` deletes the matches only.

#### When to Use

- Records systems that index Title and Subject
- Drafts that circulate inside the company
- Policies that ban Author but allow Keywords
- Fillable PDFs that must keep their form fields

#### Pros and Cons

**Pros:**
- Selective: keeps the fields that search depends on
- The rule is an explicit object that is easy to review
- The same specification can be reused in the verify step

**Cons:**
- You have to update the specification when new identity fields appear
- A Producer value can be written again by the PDF engine on save

#### Code Example

```java
Specification authorFields = new ContainsTagSpecification(Tags.getPerson().getCreator())
        .or(new ContainsTagSpecification(Tags.getPerson().getEditor()))
        .or(new WithNameSpecification("Author", true))
        .or(new WithNameSpecification("Creator", true))
        .or(new WithNameSpecification("Producer", true));

try (Metadata metadata = new Metadata(inputPath)) {
    int removed = metadata.removeProperties(authorFields);
    System.out.println(removed);
    metadata.save(outputPath);
}
```

---

### Method 3: Inspect PDF document metadata

**Overview:** Read-only listing of Author, Creator, Producer, Title, Subject and Keywords for a before/after baseline.

#### When to Use

- First step of any cleanup job
- Investigating "we thought the file was clean" tickets

#### Code Example

```java
Specification identityAndContent = new ContainsTagSpecification(Tags.getPerson().getCreator())
        .or(new ContainsTagSpecification(Tags.getTool().getSoftware()))
        .or(new ContainsTagSpecification(Tags.getContent().getTitle()))
        .or(new ContainsTagSpecification(Tags.getContent().getSubject()))
        .or(new WithNameSpecification("Author", true))
        .or(new WithNameSpecification("Creator", true))
        .or(new WithNameSpecification("Producer", true))
        .or(new WithNameSpecification("Keywords", true));

try (Metadata metadata = new Metadata(inputPath)) {
    for (MetadataProperty property : metadata.findProperties(identityAndContent)) {
        System.out.println(String.format("%s = %s", property.getName(), property.getValue()));
    }
}
```

For a typical PDF the output contains properties from both packages, for example `author` and `producer` from the Info dictionary and `dc:creator` and `dc:subject` from XMP.

---

### Method 4: Verify author metadata removed

**Overview:** Re-opens the cleaned PDF and prints `true` when no Author, `Person.Creator` or `Person.Editor` properties are left.

#### When to Use

- Assertion in CI after the cleanup
- Telling Author removal apart from engine stamps

#### Code Example

```java
Specification authorLeftovers = new ContainsTagSpecification(Tags.getPerson().getCreator())
        .or(new ContainsTagSpecification(Tags.getPerson().getEditor()))
        .or(new WithNameSpecification("Author", true));

try (Metadata metadata = new Metadata(outputPath)) {
    IReadOnlyList<MetadataProperty> leftovers = metadata.findProperties(authorLeftovers);
    System.out.println(leftovers.getCount() == 0);
}
```

{{< alert style="warning" >}}
Any change saved to a signed PDF usually invalidates its digital signatures, whichever method you use. Clean the metadata before the document is signed.
{{< /alert >}}

## Side-by-Side Feature Comparison

| Concern | sanitize() | removeProperties |
|---|---|---|
| Clears XMP packages detected by the API | Yes | Only properties matched by the specification |
| Keeps Title / Subject / Keywords | No | Yes (with the specification above) |
| Keeps form fields, attachments and bookmarks | No (since 26.11) | Yes |
| Best policy fit | Full outbound wipe | Identity redaction |

## Real-World Use Cases

### Contract sent to an external party

**Challenge:** A draft contract still shows the name of the lawyer who wrote it in Author, and Keywords contain the internal case number.

**Solution:** Inspect the file, call `sanitize()`, then run the verify step before the file is uploaded to the partner portal.

**Result:** The log contains the number of removed properties, and the verify step prints `true`.

### Fillable form in a records system

**Challenge:** An application form has to go to the archive without the name of the clerk who prepared it. The form fields must stay, and the archive indexes Title and Subject.

**Solution:** Use `removeProperties` with the author specification instead of `sanitize()`, then inspect the file to confirm that Title and Subject are still there.

**Result:** Author-style fields are removed, while the form fields and the descriptive fields stay in the document.

## Why do Creator and Producer still appear after a successful clean?

Re-open the cleaned PDF and look only for Author, `Person.Creator` and `Person.Editor`. A Creator or Producer value tagged as `Tool.Software` can be written by the PDF engine when the file is saved. It names the software, not the author of the document. Treat such values as expected and fail the check only when Author-style properties are left. This keeps the CI check honest and matches what a compliance reviewer asks after a metadata cleanup.

## FAQ

### Does sanitize() remove XMP as well as the Info dictionary?

Yes. `sanitize()` clears the metadata packages GroupDocs.Metadata detects in the PDF, including the Info dictionary and XMP. Inspect your own files before and after the cleanup to confirm it.

### Can I keep Keywords while removing Author?

Yes. Use `removeProperties` with a specification that matches only person tags and identity names, as in Method 2.

### Why does my code throw an exception on save?

Without a license GroupDocs.Metadata runs in evaluation mode, which does not save files. Apply a [license]({{< ref "metadata/java/getting-started/evaluation-limitations-and-licensing.md" >}}) or a temporary license before you run the cleanup.

### Which GroupDocs.Metadata version do these samples use?

GroupDocs.Metadata for Java **26.11**. The extended PDF cleanup of `sanitize()` (attachments, bookmarks, form fields and signatures) requires 26.11 or later.

## Conclusion

Use **sanitize()** when a PDF leaves a trust boundary and nothing identity-related, including attachments and signatures, should go with it. Use **removeProperties** when Title, Subject or form fields must stay. In both cases, inspect the file before the cleanup and verify Author after it, so operators can see the evidence.

## See Also

- [GroupDocs.Metadata for Java - clean metadata]({{< ref "metadata/java/developer-guide/basic-usage/clean-metadata.md" >}})
- [GroupDocs.Metadata for Java - remove metadata properties]({{< ref "metadata/java/developer-guide/basic-usage/remove-metadata-properties.md" >}})
- [GroupDocs.Metadata for Java - working with metadata in PDF documents]({{< ref "metadata/java/developer-guide/advanced-usage/managing-metadata-for-specific-formats/documents/working-with-metadata-in-pdf-documents.md" >}})
- [Removing PII from Office documents in Java]({{< ref "metadata/java/use-cases/remove-pii-from-office-files.md" >}})
- [API reference](https://reference.groupdocs.com/metadata/java/)
