---
id: xmp-metadata-in-adobe-psd-and-ai
url: /metadata/python-net/use-cases/xmp-metadata-in-adobe-psd-and-ai/
title: "Managing XMP in Adobe PSD and AI Files in Python: Integration Guide"
weight: 1
description: "Integrate XMP reading and writing for Photoshop PSD and Illustrator AI files into Python apps: Dublin Core fields, keywords, copyright stamping, and DAM ingestion patterns."
keywords: xmp metadata python, psd metadata, ai file metadata, dublin core, dc:subject keywords, copyright stamping, dam integration, groupdocs metadata
productName: GroupDocs.Metadata for Python via .NET
structuredData:
    showOrganization: True
toc: true
draft: false
---

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[manage-xmp-in-psd-and-ai-files-python](https://github.com/groupdocs-metadata/manage-xmp-in-psd-and-ai-files-python)
{{< /alert >}}

## Overview

XMP management is a GroupDocs.Metadata capability for Python via .NET that reads and writes the metadata packet embedded in Photoshop PSD and Illustrator AI files. The integration problem it solves is unglamorous but constant: your application receives Adobe assets and must answer who owns them, what they show, and how they should be indexed, without opening Photoshop. The data is there, inside an XML packet spread across schemes, but parsing a binary PSD to reach it is not a reasonable sprint task. With the `Metadata` class the packet is three attribute lookups away, and the same calls work on both formats. This guide takes the five functions from the linked repository and turns them into integration patterns: a full-packet snapshot for ingestion, scheme-scoped reads for API endpoints, and two write operations for ownership and search tagging. By the end you can wire each pattern into an existing service and know which one belongs where.

The guide moves from a quickstart read of Dublin Core fields, through the ingestion, stamping, and tagging patterns the repository implements, to the scheme-creation guards that make writes safe on XMP-less files. It closes with error behavior and a production checklist.

## Quickstart

Get XMP reading working in your project in minutes:

**Step 1 — Install the package**

Run `pip install groupdocs-metadata-net==26.5`, the exact version the repository pins in `requirements.txt`.

**Step 2 — Add to your project**

The shortest useful read is the Dublin Core scheme, the dc:* fields most DAM systems agree on:

```python
result: dict = {}
with Metadata(adobe_file_path) as metadata:
    root = metadata.get_root_package()
    xmp = getattr(root, "xmp_package", None)
    if xmp is None:
        return result
    dc = xmp.schemes.dublin_core
    if dc is None:
        return result
    for p in dc:
        value = (str(p.interpreted_value) if p.interpreted_value is not None
                 else (str(p.value) if p.value is not None else ""))
        result[p.name] = value
return result
```

**Step 3 — Run it**

Point the function at a PSD or AI file. You get a dict of fields like Title, Creator, Rights, and Subject; on a file with no XMP you get an empty dict, not an exception. In the repository, `python main.py` runs this against a seeded `sample.psd` and prints a PASS line per step.

## Prerequisites

- Python 3 with pip
- `groupdocs-metadata-net==26.5` installed
- A license file is optional; without one the code runs in evaluation mode, and `main.py` prints a warning instead of failing

## Core Concepts

Before wiring the patterns in, three ideas carry the whole API:

- **Root package**: `metadata.get_root_package()` returns the format-specific root; its `xmp_package` attribute is the XMP packet, or `None` when the file carries no XMP.
- **Schemes**: the packet groups values into namespaces, with `dublin_core` for interoperable fields, `photoshop` for editorial context, and `xmp_basic` for tool identity. Each is `None` until something creates it.
- **XmpArray**: multi-valued fields are typed arrays. `ORDERED` where sequence means something (creators), `UNORDERED` for bags (keywords).

## Integration Patterns

### Pattern 1: Full-packet snapshot at ingestion

When an asset enters your system, capture everything once and index it. The snapshot function walks the packet, then each named scheme, then sweeps the remaining property tree for anything nonstandard:

```python
result: dict = {}
with Metadata(adobe_file_path) as metadata:
    root = metadata.get_root_package()
    xmp = getattr(root, "xmp_package", None)
    if xmp is not None:
        for p in xmp:
            _put(result, p)

        schemes = xmp.schemes
        for scheme in (schemes.dublin_core, schemes.xmp_basic, schemes.photoshop,
                       schemes.camera_raw, schemes.paged_text, schemes.xmp_dynamic_media,
                       schemes.xmp_media_management):
            if scheme is None:
                continue
            for p in scheme:
                _put(result, p)

    for p in metadata.find_properties(lambda p: p.name is not None):
        if p.name not in result:
            _put(result, p)
return result
```

**Key points:**
- Seven schemes are read explicitly; the `find_properties` sweep catches custom packets the schemes miss.
- The `_put` helper stores values under qualified names and prefers `interpreted_value`, so dates arrive human-readable.
- One file open serves the whole snapshot, which is what you want in a bulk ingestion loop.

### Pattern 2: Ownership stamping on export

Legal wants dc:rights set, and asset tooling reads dc:creator and xmp:CreatorTool. This write covers all three, creating any missing scheme first:

```python
with Metadata(input_path) as metadata:
    root = metadata.get_root_package()
    xmp = getattr(root, "xmp_package", None)
    if xmp is None:
        root.xmp_package = XmpPacketWrapper()
        xmp = root.xmp_package
    if xmp.schemes.dublin_core is None:
        xmp.schemes.dublin_core = XmpDublinCorePackage()

    dc = xmp.schemes.dublin_core
    dc.set_rights(copyright)
    dc.set("dc:creator", XmpArray.from_([creator], XmpArrayType.ORDERED))

    if xmp.schemes.xmp_basic is None:
        xmp.schemes.xmp_basic = XmpBasicPackage()
    xmp.schemes.xmp_basic.creator_tool = creator

    metadata.save(output_path)
```

`set_rights` writes the typed dc:rights field, while `set` with an ORDERED `XmpArray` writes the creator list. Writing `creator_tool` keeps XmpBasic readers in agreement with Dublin Core. Saving to `output_path` leaves the source untouched. Sane default for an export step.

### Pattern 3: Keyword tagging for search

dc:subject is the vocabulary DAM search indexes. The function replaces the whole bag in one write:

```python
with Metadata(input_path) as metadata:
    root = metadata.get_root_package()
    xmp = getattr(root, "xmp_package", None)
    if xmp is None:
        root.xmp_package = XmpPacketWrapper()
        xmp = root.xmp_package
    if xmp.schemes.dublin_core is None:
        xmp.schemes.dublin_core = XmpDublinCorePackage()

    xmp.schemes.dublin_core.set(
        "dc:subject",
        XmpArray.from_(list(keywords), XmpArrayType.UNORDERED))
    metadata.save(output_path)
```

`UNORDERED` is the right container here. Keyword order carries no meaning to indexers, and the write replaces the whole bag, so merge with previously read keywords when you need additive tagging. The repository's `main.py` writes three sample keywords and asserts the first one lands in the saved bytes.

### Pattern 4: Editorial context for search filters

Bridge, Lightroom, and DAM filters read photoshop:* fields that Dublin Core does not carry. This read returns the eight fields the repository surfaces, empty string when a field is absent:

```python
result: dict = {}
with Metadata(adobe_file_path) as metadata:
    root = metadata.get_root_package()
    xmp = getattr(root, "xmp_package", None)
    if xmp is None:
        return result
    ps = xmp.schemes.photoshop
    if ps is None:
        return result

    def _to_str(v):
        return str(v) if v is not None else ""

    result["photoshop:ColorMode"] = _to_str(getattr(ps, "color_mode", None))
    result["photoshop:IccProfile"] = _to_str(getattr(ps, "icc_profile", None))
    result["photoshop:City"] = _to_str(getattr(ps, "city", None))
    result["photoshop:Country"] = _to_str(getattr(ps, "country", None))
    result["photoshop:DateCreated"] = _to_str(getattr(ps, "date_created", None))
    result["photoshop:CaptionWriter"] = _to_str(getattr(ps, "caption_writer", None))
    result["photoshop:Credit"] = _to_str(getattr(ps, "credit", None))
    result["photoshop:Source"] = _to_str(getattr(ps, "source", None))
return result
```

Each field is a typed property on the scheme object, read via `getattr` with a `None` default. ColorMode and IccProfile matter to print workflows; City, Country, and Credit drive editorial search. I default to scheme-scoped reads like this in request handlers and keep the Pattern 1 sweep for background ingestion. Nine fields read faster than a whole tree.

### How do I add XMP to a file that has none?

Create the missing layers before writing. Both write functions here check xmp_package and assign XmpPacketWrapper() when it is absent, then create XmpDublinCorePackage() the same way. After that, set_rights and set work exactly as they do on files with existing metadata. The pattern makes ownership stamping safe for freshly exported assets straight out of Illustrator.

## Error Handling

The read functions in this guide fail soft by design. A file without an XMP packet, or with a packet missing the requested scheme, produces an empty dict through early returns rather than raising. That behavior is worth preserving in your integration, because both cases are routine in real asset folders.

**Common situations and what they mean:**

| Situation | Cause | What to do |
|-------|-------|-----|
| Empty dict from a read | File has no XMP packet or lacks that scheme | Treat as "no metadata", not as failure |
| Written values missing after save | Output re-read from wrong path | Re-read the file at `output_path`, as `main.py` does |
| Evaluation marks in output | No license applied | Set `LICENSE_PATH` in `main.py` or ship a license in production |

## Performance Tips

All five functions open the file once and hold it inside a `with` block, which is the pattern to keep when you scale up.

- **Scope reads to a scheme:** when an endpoint needs nine fields, Pattern 4 beats the full sweep; save Pattern 1 for ingestion jobs.
- **Write once per file:** batch your dc:subject changes into one list and one `set` call instead of saving per keyword.
- **Reuse the snapshot:** the Pattern 1 dict is flat and JSON-friendly; index it instead of re-opening files to answer metadata queries.

## Production Readiness Checklist

Before deploying to production, verify the following:

- [ ] Reads treat empty dicts as valid results for XMP-less files
- [ ] Writes go to a separate output path, or overwriting originals is an explicit decision
- [ ] Keyword updates merge with existing dc:subject values where additive tagging is expected
- [ ] A license is applied so evaluation mode never touches production assets
- [ ] Error handling is implemented for all edge cases
- [ ] Logging is in place for XMP write operations
- [ ] Both PSD and AI samples from your own pipeline have been round-tripped in a test

## Frequently Asked Questions

**Q: Do the same calls work for both PSD and AI files?**
A: Yes. Both resolve their packet through `get_root_package()` and `xmp_package`, and the repository ships `sample.psd` and `sample.ai` to prove it. Freshly exported AI files often arrive with fewer populated schemes, which the early returns handle.

**Q: What happens to existing keywords when I write dc:subject?**
A: The `set` call replaces the whole array. Read the current bag first, extend the Python list, and write the merged result when you need additive behavior.

**Q: Why write xmp:CreatorTool in addition to dc:creator?**
A: Some tools read identity from the XmpBasic scheme rather than Dublin Core. Writing both keeps every reader consistent, which is why the stamping function updates the three fields together.

**Q: How do I verify a write actually persisted?**
A: Re-read the output file and check the values, the way `main.py` asserts that the copyright string and the first keyword survive in the saved bytes. A read-back after write is cheap insurance in an automated pipeline.

## Conclusion

Four patterns cover the integration surface: snapshot at ingestion, scheme reads at request time, ownership stamping and keyword tagging at export. Each rests on the packet-resolution guardwork you saw in every snippet, and each comes from a repository you can run before touching your own code. Wire in the read patterns first; they are risk-free and immediately useful.

## See Also

- [In-depth blog article about this project](https://blog.groupdocs.com/metadata/xmp-metadata-in-adobe-psd-and-ai-python-net/) – the DAM context behind these patterns
- [Working with XMP metadata](https://docs.groupdocs.com/metadata/python-net/working-with-xmp-metadata/) – reference for reading, updating, and removing XMP
- [Product documentation](https://docs.groupdocs.com/metadata/python-net/) – getting started and advanced topics
- [API Reference](https://reference.groupdocs.com/metadata/python-net/) – full API details for GroupDocs.Metadata for Python via .NET
- [GitHub repository](https://github.com/groupdocs-metadata/manage-xmp-in-psd-and-ai-files-python) – complete source code and more examples
