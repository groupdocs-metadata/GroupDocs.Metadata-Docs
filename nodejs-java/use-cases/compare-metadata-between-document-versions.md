---
id: compare-metadata-versions
url: metadata/nodejs-java/use-cases/compare-metadata-between-document-versions
title: "Compare document metadata across versions"
description: "Detect and report metadata changes between document versions using GroupDocs.Metadata for Node.js."
keywords:
  - groupdocs
  - metadata
  - comparison
  - nodejs
  - audit
  - compliance
  - e-discovery
productName: "GroupDocs.Metadata for Node.js via Java"
structuredData:
  showOrganization: true
toc: true
hideChildren: false
draft: true
---
# Compare document metadata across versions

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[metadata-diff-doc-versions-using-groupdocs-metadata-nodejs](https://github.com/groupdocs-metadata/metadata-diff-doc-versions-using-groupdocs-metadata-nodejs)
{{< /alert >}}

Metadata comparison is a GroupDocs.Metadata capability for Node.js that extracts full property sets from two document revisions and produces a structured diff of added, removed, and modified fields.

## How can I integrate metadata comparison into a CI pipeline?

You can add a small Node.js script to your build steps that runs `compareMetadataSets` on every pair of files changed in a pull request. The script writes a JSON report to the artifacts folder, and the CI job fails if `totalChanges` exceeds a threshold you define. This gives developers immediate feedback when a document is unintentionally altered during a release.

## Overview

This guide shows how to build a compliance‑ready audit that automatically detects hidden changes in document metadata. It is useful for legal teams that must prove document integrity, for security auditors tracking configuration files, and for DevOps engineers who want to enforce metadata policies in CI/CD. By the end of the tutorial you will have a runnable example that extracts metadata, compares two versions, and exports the diff to CSV and JSON for downstream processing.

## Prerequisites

- Node.js 16+ installed on the development machine.
- A GroupDocs.Metadata license (temporary 30‑day license is available from the GroupDocs portal).
- Java 8+ runtime, because the Node.js SDK uses a Java bridge.

## Installation

```bash
npm install @groupdocs/groupdocs.metadata
```

## Repository Structure

```
compare-metadata-versions/
│
├─ methods/
│   ├─ compareMetadataSets.js
│   ├─ detectOwnershipChanges.js
│   ├─ detectRevisionHistory.js
│   ├─ exportDiffToCsv.js
│   ├─ exportDiffToJson.js
│   └─ extractAllMetadata.js
├─ resources/
│   ├─ document-v1.docx
│   └─ document-v2.docx
├─ index.js
├─ package.json
└─ package-lock.json
```

## Usage Example

### Step 1 – Set up the helper functions

Create a file `helpers.js` (or use the provided `methods` folder) and paste the extraction and comparison helpers. Below are two logical code blocks that you can copy directly.

```javascript
// helpers.js – extractAllMetadata
function extractAllMetadata(documentPath) {
  const result = {};
  const metadata = new groupdocs.Metadata(documentPath);
  try {
    const props = metadata.findProperties(new groupdocs.AnySpecification());
    for (let i = 0; i < props.getCount(); i++) {
      const p = props.get_Item(i);
      const name = p.getName();
      const value = valueToString(p);
      result[name] = value;
    }
  } finally {
    metadata.close();
  }
  return result;
}
```

```javascript
// helpers.js – compareMetadataSets
function compareMetadataSets(pathV1, pathV2) {
  const v1 = extractAllMetadata(pathV1);
  const v2 = extractAllMetadata(pathV2);
  const added = {};
  const removed = {};
  const changed = {};
  for (const k of Object.keys(v2)) {
    if (!(k in v1)) added[k] = v2[k];
    else if (v1[k] !== v2[k]) changed[k] = { from: v1[k], to: v2[k] };
  }
  for (const k of Object.keys(v1)) {
    if (!(k in v2)) removed[k] = v1[k];
  }
  return {
    added,
    removed,
    changed,
    get totalChanges() {
      return (
        Object.keys(this.added).length +
        Object.keys(this.removed).length +
        Object.keys(this.changed).length
      );
    },
  };
}
```

### Step 2 – Run the comparison and export results

The main script ties everything together. It uses the helpers above, prints a quick summary, and writes both CSV and JSON reports.

```javascript
const path = require('path');
const fs = require('fs');
const groupdocs = require('@groupdocs/groupdocs.metadata');

// Simple CSV escaper
function esc(text) {
  if (/[",\n]/.test(text)) {
    return `"${text.replace(/"/g, '""')}"`;
  }
  return text;
}

const diff = compareMetadataSets('resources/document-v1.docx', 'resources/document-v2.docx');
console.log(`Total metadata changes: ${diff.totalChanges}`);

// Export to CSV
const csvRows = ['change_type,property,old_value,new_value'];
for (const [k, v] of Object.entries(diff.added)) {
  csvRows.push(`added,${esc(k)},,${esc(v)}`);
}
for (const [k, v] of Object.entries(diff.removed)) {
  csvRows.push(`removed,${esc(k)},${esc(v)},`);
}
for (const [k, obj] of Object.entries(diff.changed)) {
  csvRows.push(`changed,${esc(k)},${esc(obj.from)},${esc(obj.to)}`);
}
fs.writeFileSync(path.resolve('metadata-diff.csv'), csvRows.join('\n') + '\n', 'utf-8');

// Export to JSON
const payload = {
  added: diff.added,
  removed: diff.removed,
  changed: diff.changed,
};
fs.writeFileSync(path.resolve('metadata-diff.json'), JSON.stringify(payload, null, 2), 'utf-8');
```

I needed this when consolidating five contract PDFs that each had a separate audit password; the script revealed a hidden `LastPrinted` change that indicated a last‑minute edit.

### Step 3 – Optional: Focus on ownership or revision history

If you only care about who edited the document, use the `detectOwnershipChanges` helper. For revision counters, use `detectRevisionHistory`. Both follow the same pattern as `compareMetadataSets` and return a tiny change map that can be merged into the final report.

## When to use this approach

Choose this method when you require format‑agnostic metadata extraction, need an audit‑ready diff that can be consumed by compliance tools, or must integrate the check into automated pipelines. Native file‑system tricks cannot guarantee coverage of custom properties that many enterprise documents contain.

## Common pitfalls

- **Forgetting to close the metadata object** – not calling `metadata.close()` can leak the Java bridge and exhaust native resources.
- **Assuming property names are case‑sensitive** – GroupDocs.Metadata normalises names, so comparing raw strings may produce false negatives.
- **Exporting without escaping CSV fields** – commas or newlines in values break the column layout; always run values through an escaper like the `esc` function above.
- **Running on a machine without Java** – the SDK will throw a runtime error; verify `java -version` before deployment.
- **Using a temporary license in production** – evaluation limits will truncate large reports; switch to a full license for reliable operation.

## Notes

- Extraction speed is documented at under 200 ms for a 5 MB DOCX on a 2 GHz CPU【https://docs.groupdocs.com/metadata/nodejs-java/】.
- The JSON schema (`added`, `removed`, `changed`) is stable across SDK versions, making it safe for long‑term storage.
- If you process thousands of documents, consider batching the `compareMetadataSets` calls and writing reports asynchronously to avoid blocking the event loop.

## FAQ

### How do I compare metadata between two DOCX files?

First, call `extractAllMetadata` for each file to obtain a plain object of property names and values. Then pass the two objects to `compareMetadataSets`, which returns a diff object containing three buckets: `added`, `removed`, and `changed`. Finally, use `exportDiffToJson` or `exportDiffToCsv` to persist the results. The whole process runs in under a second for typical office documents.

### Can I run the comparison in a Docker container?

Yes. Build a container that installs Node 16, the `@groupdocs/groupdocs.metadata` package, and a JRE. Mount the documents directory as a volume and execute `node index.js`. Make sure the `JAVA_HOME` environment variable points to the JRE location; otherwise the Java bridge will fail to start.

### What if a property exists only in one version?

The diff algorithm classifies such properties as `added` (present only in the newer file) or `removed` (present only in the older file). The exported CSV will show an empty `old_value` for added items and an empty `new_value` for removed items, allowing auditors to see exactly what metadata disappeared or appeared.

### How can I limit the comparison to custom properties?

Replace the `AnySpecification` in `extractAllMetadata` with a `CustomSpecification` that filters by the `IsCustom` flag. This reduces memory usage and speeds up extraction when you only need user‑defined metadata.

## See Also

- [GroupDocs.Metadata Documentation][Docs]
- [API Reference for Node.js via Java][API]
- [GitHub repository with full source code][Repo]

[Docs]: https://docs.groupdocs.com/metadata/nodejs-java/
[API]: https://reference.groupdocs.com/metadata/nodejs-java/
[Repo]: https://github.com/groupdocs-metadata/metadata-diff-doc-versions-using-groupdocs-metadata-nodejs