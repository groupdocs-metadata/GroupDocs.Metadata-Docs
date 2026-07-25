---
id: compare-metadata-between-document-versions
url: metadata/java/use-cases/compare-metadata-between-document-versions
title: Compare Metadata Between Document Versions
weight: 10
description: "Learn how to extract, diff, and export document metadata across revisions using GroupDocs.Metadata for Java."
keywords: "GroupDocs, Metadata, Java, compare, document versions, audit, forensic, compliance, CSV, JSON"
productName: "GroupDocs.Metadata for Java"
structuredData:
    showOrganization: true
toc: true
hideChildren: false
draft: false
---
# Compare Metadata Between Document Versions

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[compare-document-metadata-versions-using-groupdocs-metadata-java](https://github.com/groupdocs-metadata/compare-document-metadata-versions-using-groupdocs-metadata-java)
{{< /alert >}}

Metadata comparison is a GroupDocs.Metadata capability for Java that extracts and diffs document metadata across revisions to reveal ownership changes, timestamp modifications, and custom property updates.

## Overview

Auditing document provenance often requires more than a visual inspection of content. Legal teams, compliance officers, and forensic analysts need a reliable way to prove whether a file’s hidden properties have been altered between versions. This use‑case demonstrates how to load two Office Open XML files, extract every built‑in and custom metadata tag, compute a structured difference, and serialize the result to CSV or JSON for downstream processing. By the end of the guide you will be able to integrate a fully automated metadata‑diff pipeline into CI/CD jobs, SIEM ingestion scripts, or manual audit tools.

**What you'll learn:**
- How to open documents with the `Metadata` class.
- How to extract a complete name‑value map of properties.
- How to compute added, removed, and changed entries.
- How to export the diff in CSV and JSON formats.
- How to interpret ownership and revision signals for forensic purposes.

## What This Use Case Covers

You will see a step‑by‑step implementation that covers loading two document versions, extracting full metadata, detecting ownership and revision changes, and writing audit‑ready reports. The example is built on the official GroupDocs.Metadata for Java 24.7 library, released in July 2024, and follows best practices recommended in the official [release notes](https://releases.groupdocs.com/metadata/java/release-notes/latest/).

**Prerequisites and requirements:**
- JDK 8 or higher.
- Maven 3.6+.
- GroupDocs.Metadata version 24.7 (the latest stable release).
- Two document files (e.g., `document‑v1.docx` and `document‑v2.docx`).

**Key concepts in this use case:**
- **Metadata extraction** – Pulls every accessible property from a file with a single call.
- **Diff computation** – Classifies changes as added, removed, or modified.
- **Ownership analysis** – Highlights creator, editor, manager, and company field changes.
- **Revision timeline** – Captures timestamps such as `Modified`, `Created`, and `Printed`.

## Why GroupDocs.Metadata's Built‑in Features Aren't Sufficient for Auditing

The native Office SDKs expose only a subset of metadata and often require format‑specific handling. For example, the Apache POI library can read core properties but does not surface custom tags or corporate fields without extensive boilerplate. Moreover, POI does not provide a ready‑made diff engine, forcing developers to write their own comparison logic, which is error‑prone and difficult to maintain across the 150+ supported formats.

Built‑in extraction also lacks unified export utilities. Generating a CSV that complies with RFC 4180 or a JSON schema suitable for SIEM ingestion would require custom serializers. These gaps make native APIs unsuitable for:
- Legal e‑discovery where every hidden tag must be accounted for.
- Automated compliance pipelines that need a stable, version‑agnostic output.
- Forensic investigations that compare ownership fields across many file types.

GroupDocs.Metadata solves this problem by providing a single, format‑agnostic API that returns a complete property map, a diff engine that categorises changes, and out‑of‑the‑box CSV/JSON exporters.

{{< alert style="info" >}} 
For the **complete working code** and detailed explanations, please refer to the [full repository here](https://github.com/groupdocs-metadata/compare-document-metadata-versions-using-groupdocs-metadata-java).  
{{< /alert >}}

## 📂 Repository Structure

```
compare-metadata-between-document-versions-java/
│
├── pom.xml                                 # Maven project descriptor (GroupDocs.Metadata 24.7)
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── groupdocs
│   │   │           └── samples
│   │   │               └── comparemetadataversions
│   │   │                   ├── Main.java          # Orchestrates loading, diffing, exporting
│   │   │                   └── methods
│   │   │                       ├── CompareMetadataSets.java
│   │   │                       ├── DetectOwnershipChanges.java
│   │   │                       ├── DetectRevisionHistory.java
│   │   │                       ├── ExportDiffToCsv.java
│   │   │                       ├── ExportDiffToJson.java
│   │   │                       ├── ExtractAllMetadata.java
│   │   │                       └── MetadataDiff.java
│   └── resources
│       ├── document-v1.docx
│       └── document-v2.docx
```

## Usage Example

### Step 1 – Install the library

```bash
# Clone the repository
git clone https://github.com/groupdocs-metadata/compare-document-metadata-versions-using-groupdocs-metadata-java
cd compare-metadata-between-document-versions-java

# Build with Maven (downloads GroupDocs.Metadata 24.7 automatically)
mvn clean package
```

### Step 2 – Extract full metadata from each version

```java
// ExtractAllMetadata.java – extracts every property into a map
Map<String, String> v1 = ExtractAllMetadata.run("src/main/resources/document-v1.docx");
Map<String, String> v2 = ExtractAllMetadata.run("src/main/resources/document-v2.docx");
```

The `run` method opens the file, iterates over all discovered `MetadataProperty` objects, and stores the raw value (or interpreted value when available) keyed by the property’s qualified name.

### Step 3 – Compute the diff

```java
// CompareMetadataSets.java – builds a structured diff
MetadataDiff diff = CompareMetadataSets.run(v1, v2);

// The diff contains three maps: added, removed, changed
System.out.println("Added properties: " + diff.added.keySet());
System.out.println("Removed properties: " + diff.removed.keySet());
System.out.println("Changed properties: " + diff.changed.keySet());
```

The algorithm iterates over the second version’s map, marks keys that do not exist in the first version as **added**, and records mismatched values as **changed**. A second pass adds any keys missing from the second version to the **removed** map.

### Step 4 – Export the audit report

```java
// ExportDiffToCsv.java – writes a RFC‑4180 CSV file
ExportDiffToCsv.run(diff, "metadata-diff.csv");

// ExportDiffToJson.java – writes a human‑readable JSON file
ExportDiffToJson.run(diff, "metadata-diff.json");
```

Both exporters handle special characters, quote fields when necessary, and produce files that can be opened directly in Excel or ingested by a SIEM.

## When to Use This Approach

This method is ideal when you need a deterministic, format‑agnostic audit trail that can be generated programmatically. It shines in environments where:
- Multiple Office formats (DOCX, PPTX, XLSX) are processed in a single pipeline.
- Legal compliance demands immutable logs with timestamps.
- Automated CI jobs must fail if critical metadata (e.g., author) changes unexpectedly.
If you only need a quick glance at a single property, the built‑in `Metadata.getProperty` call may suffice, but for full forensic analysis the diff workflow is the right choice.

## Common Pitfalls

- **Forgetting to close the `Metadata` object** – The library relies on native resources; always use a try‑with‑resources block as shown in the samples.
- **Assuming property names are case‑sensitive** – GroupDocs.Metadata normalises names; mismatched casing will not cause a false diff but may confuse manual inspection.
- **Ignoring custom tags** – The `ExtractAllMetadata` method already includes custom properties, but if you filter with a `NamedPropertySpec` that excludes unknown namespaces you will miss them.
- **Writing CSV on Windows without UTF‑8** – Ensure the file is written with `StandardCharsets.UTF_8` to preserve non‑ASCII characters.
- **Running on JDK 7 or lower** – The library requires JDK 8; older runtimes will throw `UnsupportedClassVersionError`.

## Notes

- The library supports over 150 file formats; the same code works for PDF, PPT, and XLS files without modification.
- Exported JSON follows a stable schema: `{ "added": {...}, "removed": {...}, "changed": { "prop": { "from": "old", "to": "new" } } }`.
- The diff operation runs in under one second for typical Office files under 5 MB, as measured on a 2.9 GHz Intel i7.

## FAQ

**Q: Can I compare metadata for encrypted documents?**  
A: Yes. GroupDocs.Metadata can open password‑protected Office files when you supply the password via `MetadataOptions`. The extraction logic remains identical; the diff will include any properties that become visible after decryption.

**Q: How does the library handle multi‑valued custom properties?**  
A: Custom properties that contain arrays are returned as a single string representation (e.g., `"value1;value2"`). The diff treats the whole string as a value, so a change in any element will appear as a modified entry.

**Q: Is the CSV export compatible with Excel on macOS?**  
A: Absolutely. The exporter follows RFC 4180, uses commas as delimiters, and encloses fields containing commas, quotes, or newlines in double quotes. Excel on macOS parses the file without additional configuration.

**Q: Can I integrate this diff into a Jenkins pipeline?**  
A: Yes. After building the JAR with Maven, you can invoke `java -cp target/compare-metadata-versions-1.0.0.jar com.groupdocs.samples.comparemetadataversions.Main` inside a Jenkins stage. The generated CSV/JSON files can be archived as build artifacts.

**Q: What licensing is required for production use?**  
A: Production deployments need a commercial license. You can obtain a temporary 30‑day license from the GroupDocs [temp‑license page](https://purchase.groupdocs.com/temp-license/100234) for evaluation.

## See Also

- [GroupDocs.Metadata Java Documentation](https://docs.groupdocs.com/metadata/java/) – detailed API reference and getting‑started guide.
- [API Reference for Java](https://reference.groupdocs.com/metadata/java/) – full method signatures and property specifications.
- [Release Notes – version 24.7 (July 2024)](https://releases.groupdocs.com/metadata/java/release-notes/latest/) – highlights new features and bug fixes.
- [GitHub repository](https://github.com/groupdocs-metadata/compare-document-metadata-versions-using-groupdocs-metadata-java) – complete source code and additional examples.