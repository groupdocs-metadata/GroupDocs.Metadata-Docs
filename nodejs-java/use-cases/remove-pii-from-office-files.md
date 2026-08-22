---
id: remove-pii-from-office-files
url: /metadata/nodejs-java/use-cases/remove-pii-from-office-files/
title: 6 Methods to Remove PII from Office Document Metadata in Node.js
weight: 1
description: "Six working Node.js methods for clearing author names, comment threads, revision trails, and SharePoint fields from Office metadata with GroupDocs.Metadata, plus the leak scan that verifies the result."
keywords: remove pii, document sanitization, office metadata, nodejs metadata api, metadata leak check, gdpr documents, clean docx properties, groupdocs metadata, node.js via java
productName: GroupDocs.Metadata for Node.js via Java
structuredData:
    showOrganization: True
toc: true
draft: false
---

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[office-metadata-pii-cleanup-nodejs](https://github.com/groupdocs-metadata/office-metadata-pii-cleanup-nodejs)
{{< /alert >}}

## Overview

Metadata PII removal is a GroupDocs.Metadata capability for Node.js via Java that deletes identity-bearing properties from Word, Excel, and PowerPoint files and then verifies that they are gone. Office applications write this data without asking: the account name lands in `Author` and `LastSavedBy` on every save, revision counters and `TotalEditingTime` accumulate as the file is edited, and a document that passed through SharePoint returns with approver IDs and workflow paths attached. None of it renders on the page, so it survives every proofread and travels with the attachment.

Six methods exist here rather than one because "remove personal data" means different things at different points in a document's life. A file still moving between reviewers needs its Title and Subject intact; a file going to an external auditor does not. This page walks through each method, the property group it targets, and the conditions that make it the right choice.

**What you'll learn:**
- Which property groups an Office file carries and which rule shape fits each one
- How tag specifications differ from name specifications in the Node.js binding
- Why the verification scan belongs in the same script as the cleanup

## What This Use Case Covers

Every code block on this page comes from a runnable project that seeds a DOCX with author, comment, revision, and SharePoint properties, applies each method in turn, and asserts the outcome. The project prints an affected-property count per method, which is the number worth writing to an audit log. By the end you will be able to drop any of the six functions into an upload handler, an export job, or a CLI without modification.

**Prerequisites and requirements:**
- Node.js with a Java runtime available on the machine, since the package is Node.js via Java
- `npm install @groupdocs/groupdocs.metadata` (the project pins 26.7 and overrides `nan` to `^2.22.0`)
- A test document that has actually been through a review cycle

## Why Word's Built-in Document Inspector Isn't Enough

Word ships an inspector, and for a single file it works. Open the document, choose Check for Issues, tick the categories, click Remove All. The trouble starts one step later. The inspector is interactive, so it cannot run inside a Node.js service. It is all-or-nothing per category, so removing document properties also removes the Title a records index depends on. And it reports nothing a program can read, so nobody can answer later which fields left a given file.

This makes the built-in inspector unsuitable for:
- Upload handlers that must sanitize before storing an attachment
- Nightly jobs over an export folder
- Any workflow where the removal has to be evidenced rather than asserted

GroupDocs.Metadata solves this by exposing one property search engine: a specification object selects properties, `removeProperties` deletes the matches and returns a count, and `findProperties` runs the same selection read-only.

## 📂 Repository Structure

```
office-metadata-pii-cleanup-nodejs/
│
├── package.json                          # dependency and start script
├── index.js                              # entry point: runs all six methods
├── methods/
│   ├── removeAuthorAndCompany.js         # identity fields by tag
│   ├── clearComments.js                  # comment and reviewer fields
│   ├── stripRevisionHistory.js           # editing timeline
│   ├── removeDocumentServerProperties.js # SharePoint workflow fields
│   ├── sanitizeAllPii.js                 # one-call full wipe
│   ├── runLeakCheck.js                   # verification scan
│   └── nameContainsSpec.js               # shared substring specification
├── resources/                            # seeded input document
└── output/                               # five cleaned copies
```

## Method 1: Remove author and company fields

**Scope:** narrow | **Difficulty:** low | **Best For:** documents that stay in circulation

Four tags cover the properties that name real people and organizations: creator, editor, manager, and the corporate company field.

### How It Works

`ContainsTagSpecification` matches a property by the tag attached to it rather than by its name, and `.or()` merges the four into one specification. Tags describe meaning, so the same rule catches the equivalent field when it arrives from a different metadata package or a different format.

### Implementation

```javascript
const T = groupdocs.Tags;
const spec = new groupdocs.ContainsTagSpecification(T.getPerson().getCreator())
  .or(new groupdocs.ContainsTagSpecification(T.getPerson().getEditor()))
  .or(new groupdocs.ContainsTagSpecification(T.getPerson().getManager()))
  .or(new groupdocs.ContainsTagSpecification(T.getCorporate().getCompany()));
const affected = metadata.removeProperties(spec);
metadata.save(outputPath);
```

### Considerations

Title, Subject, and Keywords survive this pass, which is the entire point of using it instead of a full sanitize. Write the returned count to your log; a zero on a document you know is dirty usually means the input was not recognized.

## Method 2: Build the shared name specification

**Scope:** infrastructure | **Difficulty:** low | **Best For:** the three passes that match on names

### How It Works

`WithNameSpecification(needle, false)` matches any property whose name contains the needle; the `false` turns off full-name matching. The builder chains one instance per substring with `.or()` and returns a single specification.

### Implementation

```javascript
let spec = null;
for (const needle of needles) {
  const s = new groupdocs.WithNameSpecification(needle, false /* fullMatch */);
  spec = spec ? spec.or(s) : s;
}
return spec;
```

### Considerations

Because the result is an object rather than a callback, the same instance can be passed to `removeProperties` and later to `findProperties`, which keeps the verification honest.

## Method 3: Clear comment and reviewer fields

**Scope:** narrow | **Difficulty:** low | **Best For:** minutes and policies after an internal review

### How It Works

Three substrings, `Comment`, `Reviewer`, and `Reviewed`, cover reviewer names, review timestamps, and thread counters. Substring matching also catches variants such as `CommentsCount`.

### Implementation

```javascript
const affected = metadata.removeProperties(
  nameContainsSpec(['Comment', 'Reviewer', 'Reviewed']));
metadata.save(outputPath);
```

### Considerations

This clears comment *metadata*. Comment balloons stored in the document body are content, not metadata, and need a content-editing library to remove.

## Method 4: Strip the editing timeline

**Scope:** narrow | **Difficulty:** low | **Best For:** filings where drafting history is confidential

### How It Works

`Revision`, `TrackedChange`, `LastPrinted`, `TotalEditingTime`, and `EditTime` together say how many drafts existed, who edited them, how long the work took, and when it was printed.

### Implementation

```javascript
const affected = metadata.removeProperties(nameContainsSpec([
  'Revision', 'TrackedChange', 'LastPrinted', 'TotalEditingTime', 'EditTime',
]));
metadata.save(outputPath);
```

### Considerations

Both `EditTime` and `TotalEditingTime` appear deliberately, because the name differs between the packages Word writes into the same file.

## Method 5: Remove SharePoint and document-server fields

**Scope:** narrow | **Difficulty:** low | **Best For:** exports from a document management system

### How It Works

Server-managed properties carry workflow paths, approver identifiers, content-type URIs, and template references that sketch the organization behind the file.

### Implementation

```javascript
const affected = metadata.removeProperties(nameContainsSpec([
  'Server', 'Workflow', 'Approver', 'ContentType', 'Template',
]));
metadata.save(outputPath);
```

### Considerations

Field names vary between server versions. Confirm the affected count against documents from your own tenant before treating this substring list as complete.

## Method 6: Sanitize everything, then verify

**Scope:** total | **Difficulty:** low | **Best For:** the copy that leaves the organization

### How It Works

`sanitize()` clears every metadata package the library detects, custom OOXML parts included, and returns the number of properties removed. The leak check then re-opens the saved copy, runs the identity tags plus a name specification for comment and revision fields, and collects whatever still holds a value.

### Implementation

```javascript
const affected = metadata.sanitize();
metadata.save(outputPath);
```

The scan walks the search result by index, because `findProperties` returns a Java collection rather than a JavaScript array:

```javascript
const nameSpec = nameContainsSpec(['Comment', 'Reviewer', 'Revision', 'TrackedChange']);
const props = metadata.findProperties(tagSpec.or(nameSpec));
for (let i = 0; i < props.getCount(); i++) {
  const p = props.get_Item(i);
  const val = p.getValue && p.getValue();
  const value = val ? String(val.getRawValue ? val.getRawValue() : val) : '';
  if (!value || value === '0' || value === '0.0') continue;
  leaks.push(`${p.getName()}=${value}`);
}
```

### Considerations

The empty-and-zero filter matters: without it a cleared counter reads as a leak and fails a run that worked. I lost an hour to exactly that before adding it. `index.js` asserts the returned array has zero entries, so a residual property stops the script with a non-zero exit code.

## Choosing the Right Cleanup Method

| Situation | Method | Descriptive fields kept |
|---|---|---|
| Draft shared with an external reviewer | Method 1 | yes |
| Minutes published after internal review | Method 3 | yes |
| Filing with confidential drafting history | Method 4 | yes |
| Export from a SharePoint library | Method 5 | yes |
| Publication to an open portal | Method 6 | no |

### Do I need the leak check if I already call sanitize()?

Yes. Sanitize clears the packages the library detects at that moment, but templates and document servers write properties back whenever another tool reopens the file. The scan is the only step that reports the document's actual state rather than the intent of the previous call, and it costs one extra file open per document.

## Common Questions

**Why does every function close the metadata object in a `finally` block?**
The Node.js package wraps the Java API, and the underlying object holds the file open until `close()` runs. In a loop over an export folder, skipping the close exhausts file handles long before the job finishes.

**Can I merge several methods into one pass?**
Yes. Specifications compose with `.or()`, exactly as the leak check does when it joins the tag specification to the name specification. Merge them when throughput matters; keep them separate when the audit log needs a count per property group.

**Does this work on XLSX and PPTX?**
The tag-based pass carries over unchanged. The substring passes apply as well, though the exact field names depend on the producing application, so verify the counts against your own files first.

## Summary and When to Use Which Method

Start with Method 1 on anything leaving your team, add Methods 3 through 5 as your document sources demand them, and reserve Method 6 for the copy that crosses the trust boundary. Run the leak scan after whichever combination you pick, and treat a non-empty result as a failed job rather than a warning. Clone the repository, point it at one of your own reviewed documents, and read the counts before deciding which passes your pipeline needs.

## See Also

- [In-depth blog article about this project](https://blog.groupdocs.com/metadata/remove-pii-from-office-files-nodejs-java/) – the same six methods built up step by step
- [Remove metadata properties](https://docs.groupdocs.com/metadata/nodejs-java/remove-metadata-properties/) – reference for specification-driven removal
- [Find metadata properties](https://docs.groupdocs.com/metadata/nodejs-java/find-metadata-properties/) – reference for the search the leak check uses
- [Product documentation](https://docs.groupdocs.com/metadata/nodejs-java/) – getting started and advanced topics
- [API Reference](https://reference.groupdocs.com/metadata/nodejs-java/) – full API details for GroupDocs.Metadata for Node.js via Java
