---
id: remove-pii-from-office-files
url: /metadata/java/use-cases/remove-pii-from-office-files/
title: "Removing PII from Office Documents in Java: Integration Guide"
weight: 1
description: "Wire metadata PII removal into a Java service with GroupDocs.Metadata: tag specifications for identity fields, name specifications for field families, a full sanitize, and a verification scan that proves the result."
keywords: remove pii, document sanitization, office metadata, java metadata api, metadata leak check, gdpr documents, clean docx properties, groupdocs metadata, integration, production
productName: GroupDocs.Metadata for Java
structuredData:
    showOrganization: True
toc: true
draft: false
---

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[remove-pii-from-office-metadata-java](https://github.com/groupdocs-metadata/remove-pii-from-office-metadata-java)
{{< /alert >}}

## Overview

Metadata PII removal is a GroupDocs.Metadata capability for Java that deletes identity-bearing properties from Word, Excel, and PowerPoint files and reports what survived. The integration problem is placement: sanitization sits inside something that already exists, usually an export endpoint, a nightly records job, or the step that attaches a file to an outgoing message. That host code has a file path, an audit log, and a failure mode, so the API has to hand back numbers instead of working silently.

Java developers hit one API detail early. `removeProperties` takes a `Specification`, not a lambda, so every rule in this guide is an object, and rules compose through `.or(...)`. That is a small difference from other platforms with a real consequence: the specification instances are reusable, so the same rule that removes properties can be handed to `findProperties` for verification.

This guide covers four integration points: removing identity properties by semantic tag rather than by name, clearing field families such as comments, revisions, and server fields with a reusable name specification, the full `sanitize()` call and when it replaces the targeted passes, and the verification step that reads the file back and classifies whatever is left.

## Quickstart

**Step 1: add the dependency**

The library resolves from the GroupDocs Java repository, so both the repository and the dependency go into `pom.xml`:

```xml
<repository>
    <id>GroupDocsJavaAPI</id>
    <url>https://releases.groupdocs.com/java/repo/</url>
</repository>
<dependency>
    <groupId>com.groupdocs</groupId>
    <artifactId>groupdocs-metadata</artifactId>
    <version>24.7</version>
</dependency>
```

**Step 2: call sanitize on a copy**

The shortest path to a clean file is the built-in sanitize. It clears every metadata package the library detects and returns how many properties went away:

```java
try (Metadata metadata = new Metadata(inputPath)) {
    if (metadata.getFileFormat() == FileFormat.Unknown) return 0;
    int affected = metadata.sanitize();
    metadata.save(outputPath);
    return affected;
}
```

**Step 3: check the number**

Run it against a document that has been through a review cycle and write the returned count to your audit log. A zero on a file you expected to be dirty means the format was not recognized, not that the file was clean.

## Prerequisites

- JDK 8 or newer; the sample project compiles with source and target level 8
- Maven, with the GroupDocs Java repository reachable from your build environment
- GroupDocs.Metadata for Java 24.7 or later, plus a license file for production use

## Core Concepts

Three ideas carry the whole integration. A `Specification` is the predicate object that both `removeProperties` and `findProperties` accept: `ContainsTagSpecification` matches by tag, while a `Specification` subclass matches anything you can express in Java, such as a name substring. Tags classify properties by meaning, so `Tags.getPerson().getCreator()`, `getEditor()`, `getManager()`, and `Tags.getCorporate().getCompany()` keep working when the same concept arrives under a different name in another format. And every removal call returns an affected count, which is the audit-friendly output; verification is a separate read of the saved file.

### Which properties count as metadata leaks?

A metadata leak is a property that carries identity or organizational data and still holds a non-empty value after cleanup: an author name, a company, a manager, a server URL, a classification field. Counters and empty strings are excluded, because a zeroed revision counter is a cleared field. Word comments and tracked-change authors are reported separately, since they live in body content rather than metadata.

## Integration Patterns

### Pattern 1: Identity pass on the way out

Use this while a document is still in circulation and the descriptive fields have to survive. Four tag specifications joined with `.or(...)` cover creator, editor, manager, and company, leaving Title, Subject, and Keywords for the search index.

```java
try (Metadata metadata = new Metadata(inputPath)) {
    if (metadata.getFileFormat() == FileFormat.Unknown) return 0;
    int affected = metadata.removeProperties(
            new ContainsTagSpecification(Tags.getPerson().getCreator())
                .or(new ContainsTagSpecification(Tags.getPerson().getEditor()))
                .or(new ContainsTagSpecification(Tags.getPerson().getManager()))
                .or(new ContainsTagSpecification(Tags.getCorporate().getCompany())));
    metadata.save(outputPath);
    return affected;
}
```

Tag matching is format-independent, so the same call handles DOCX, XLSX, and PPTX inputs. Keep the output path different from the input path if the original has to survive for dispute resolution.

### Pattern 2: Field families by name

Comment threads, revision counters, and server-managed fields have no tag. `NameContainsSpec` takes a varargs substring list and matches any property whose name contains one of them, so a family clears in one call:

```java
try (Metadata metadata = new Metadata(inputPath)) {
    if (metadata.getFileFormat() == FileFormat.Unknown) return 0;
    int affected = metadata.removeProperties(
            new NameContainsSpec("Comment", "Reviewer", "Reviewed"));
    metadata.save(outputPath);
    return affected;
}
```

The server pass follows the same shape with a different substring list. It matters for any file that came off SharePoint, where approver IDs and content-type URIs describe internal structure:

```java
try (Metadata metadata = new Metadata(inputPath)) {
    if (metadata.getFileFormat() == FileFormat.Unknown) return 0;
    int affected = metadata.removeProperties(new NameContainsSpec(
            "Server", "Workflow", "Approver", "ContentType", "Template"));
    metadata.save(outputPath);
    return affected;
}
```

One specification class covers three passes, and the substring list is the only thing that changes between them. Substring matching also catches variants such as `CommentsCount` that an exact-name list would miss.

### Pattern 3: Wipe at the boundary, then verify

At the boundary, `sanitize()` replaces the targeted passes and verification runs the union of every rule through `findProperties`, which reads without writing. The first block assembles that predicate:

```java
LeakReport report = new LeakReport();
try (Metadata metadata = new Metadata(path)) {
    if (metadata.getFileFormat() == FileFormat.Unknown) return report;
    for (MetadataProperty p : metadata.findProperties(
            new ContainsTagSpecification(Tags.getPerson().getCreator())
                .or(new ContainsTagSpecification(Tags.getPerson().getEditor()))
                .or(new ContainsTagSpecification(Tags.getPerson().getManager()))
                .or(new ContainsTagSpecification(Tags.getCorporate().getCompany()))
                .or(new NameContainsSpec(
                        "Comment", "Reviewer", "Revision", "TrackedChange",
                        "Classification", "Department", "Server", "Workflow")))) {
```

The second block classifies each hit. Empty values and zero counters are skipped, and wrapper entries over body content go to a separate list so the pass/fail signal stays meaningful:

```java
        String value = "";
        if (p.getValue() != null && p.getValue().getRawValue() != null) {
            value = String.valueOf(p.getValue().getRawValue());
        }
        if (value.isEmpty() || value.equals("0") || value.equals("0.0")) continue;
        String entry = p.getName() + "=" + value;
        String name = p.getName() == null ? "" : p.getName();
        if (name.startsWith("Comment") || name.startsWith("Revision")
                || name.startsWith("Inspection")) {
            report.contentLevelLeaks.add(entry);
        } else {
            report.metadataLeaks.add(entry);
        }
    }
}
return report;
```

Run the scan against the saved copy rather than the source, or it reports the input file's state. Treat a non-empty `metadataLeaks` list as a failure condition worth throwing on, and read `contentLevelLeaks` as informational: those entries need a content-editing library such as Aspose.Words.

## Error Handling

Two failure modes matter in a service. An unreadable file yields `FileFormat.Unknown`, which every operation checks before touching properties, and try-with-resources closes the handle even when the save throws.

```java
try (Metadata metadata = new Metadata(inputPath)) {
    if (metadata.getFileFormat() == FileFormat.Unknown) return 0;
    int affected = metadata.removeProperties(new NameContainsSpec(
            "Revision", "TrackedChange", "LastPrinted", "TotalEditingTime", "EditTime"));
    metadata.save(outputPath);
    return affected;
}
```

**Common errors and how to fix them:**

| Symptom | Cause | Fix |
|---|---|---|
| Affected count is 0 on a file you know is dirty | format not recognized, so the guard returned early | check `getFileFormat()` before assuming the document was clean |
| Evaluation-mode watermarking in the output | license file missing or path wrong | point the license path at a real `.lic` before the first `Metadata` construction |
| Leak check reports the same value you removed | the scan ran against the input path | pass the saved output path to the verification call |

## Performance Tips

The removal work is cheap; opening and rewriting the file is not. I learned that running the four passes over an export folder, where the repeated opens dominated the runtime rather than the predicates. Merging specifications into one `.or(...)` chain gives one open and one save instead of four, and the leak check already shows that shape.

One pass with a combined specification beats four sequential passes over the same document, and specification instances hold no per-file state, so a static instance can serve every file in a loop.

## Production Readiness Checklist

Before deploying, verify the following:

- [ ] Sanitization writes to a new path and the original is retained per your retention policy
- [ ] The leak check runs on the saved copy of every processed file
- [ ] A non-empty metadata-leak list fails the job rather than logging a warning
- [ ] Affected counts are written to the audit trail per file and per operation
- [ ] The license is applied once at startup, not per file

## Frequently Asked Questions

**Q: Do I still need the leak check if I call sanitize()?**
A: Yes, and the sample keeps it for that case. Sanitize clears the packages the library detects, but templates and document servers write properties back when another tool in the pipeline reopens the file. The scan is the only step that reports state rather than intent.

**Q: Does this work on XLSX and PPTX as well as DOCX?**
A: The tag-based identity pass carries over unchanged, because tags classify properties across formats. The substring passes apply too, though field names differ per producing application, so validate the counts against your own files.

**Q: Why are Word comments still visible after a full sanitize?**
A: Comment text and tracked-change authors live in `word/document.xml`, which is document body content rather than a metadata package. GroupDocs.Metadata reports them through the content-level list but does not edit them; removing them requires a content-editing library such as Aspose.Words.

## Conclusion

Three shapes cover the practical range: a tag pass for identity fields while a document is in use, name-based passes for the families tags do not classify, and a full sanitize plus verification at the boundary. Wire the affected counts into the audit log, treat a non-empty metadata-leak list as a failure, and the pipeline reports facts instead of intentions.

## See Also

- [In-depth blog article about this project](https://blog.groupdocs.com/metadata/remove-pii-from-office-files-java/) – business context and the full pipeline
- [Remove metadata properties](https://docs.groupdocs.com/metadata/java/remove-metadata-properties/) – reference for specification-driven removal
- [Clean metadata](https://docs.groupdocs.com/metadata/java/clean-metadata/) – what a full sanitize covers
- [Product documentation](https://docs.groupdocs.com/metadata/java/) – getting started and advanced topics
- [API Reference](https://reference.groupdocs.com/metadata/java/) – full API details for GroupDocs.Metadata for Java
- [GitHub repository](https://github.com/groupdocs-metadata/remove-pii-from-office-metadata-java) – complete source code and more examples
