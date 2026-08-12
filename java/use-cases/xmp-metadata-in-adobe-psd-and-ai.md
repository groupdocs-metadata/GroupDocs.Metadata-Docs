---
id: xmp-metadata-in-adobe-psd-and-ai
url: /metadata/java/use-cases/xmp-metadata-in-adobe-psd-and-ai/
title: 5 XMP Operations for PSD and AI Files in Java - Complete Comparison Guide
weight: 1
description: "Compare five ways to work with XMP metadata in Photoshop PSD and Adobe Illustrator files using GroupDocs.Metadata for Java: snapshot, scheme reads, and guarded writes."
keywords: xmp metadata, psd metadata, adobe illustrator metadata, java xmp, dublin core, dc:subject keywords, copyright stamping, groupdocs metadata java
productName: GroupDocs.Metadata for Java
structuredData:
    showOrganization: True
toc: true
draft: true
---

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[read-and-write-xmp-in-psd-ai-files-java](https://github.com/groupdocs-metadata/read-and-write-xmp-in-psd-ai-files-java)
{{< /alert >}}

## Introduction

XMP management is a GroupDocs.Metadata capability for Java that reads and writes the metadata packet inside Photoshop PSD and Adobe Illustrator AI files. Five operations exist in the companion repository because integrations need xmp metadata at different depths. Ingestion wants everything in one pass. A licensing check wants one field, fast. Search wants keywords written reliably, and compliance wants ownership stamped even on files that arrive carrying no packet at all. Each operation answers one of those calls, and this guide compares them so you can pick the subset your system needs.

Every code block below comes from a runnable Maven project that seeds `sample.psd` and `sample.ai` and asserts each step, so the comparison is grounded in code you can execute before adopting it.

## What This Guide Covers

You will see when the full snapshot beats scheme-scoped reads, how the two write operations guard-create missing schemes, and what empty results mean on sparse adobe illustrator metadata. The layout is small: `Main.java` drives five methods from the `methods` package against seeded resources and writes stamped copies into `output/`.

**Prerequisites:**
- JDK 8 or later with Maven
- `groupdocs-metadata` 24.7 from the GroupDocs repository declared in `pom.xml`

## Quick Decision Matrix

| Scenario | Recommended Operation | Why |
|----------|----------------------|-----|
| Asset enters the DAM | Full snapshot | One pass captures every property |
| Licensing gate checks dc:rights | Dublin Core read | Nine fields, no tree walk |
| Search filters need editorial context | Photoshop scheme read | Typed getters for the photoshop:* fields |
| Ownership must be stamped at export | Copyright and creator write | Three layers updated, schemes created if absent |
| Assets need to be findable | Keyword write | dc:subject bag written the way indexers read it |

## Detailed Operation Analysis

### Operation 1: Full-packet snapshot

**Overview:** dumps every XMP property of an Adobe file into one `LinkedHashMap`, preserving declaration order.

#### How It Works

The method casts `getRootPackage()` to `IXmp`, iterates the packet, walks seven named schemes through a private `collect` helper, and finishes with a `findProperties` sweep driven by a small `Specification` subclass. The `put` helper prefers `getInterpretedValue()`, so dates and enumerations arrive readable.

#### Implementation

```java
Map<String, String> result = new LinkedHashMap<>();
try (Metadata metadata = new Metadata(adobeFilePath)) {
    IXmp root = (IXmp) metadata.getRootPackage();
    if (root != null && root.getXmpPackage() != null) {
        for (MetadataProperty p : root.getXmpPackage()) {
            put(result, p);
        }
        XmpSchemes schemes = root.getXmpPackage().getSchemes();
        collect(result, schemes.getDublinCore());
        collect(result, schemes.getXmpBasic());
        collect(result, schemes.getPhotoshop());
        collect(result, schemes.getCameraRaw());
        collect(result, schemes.getPagedText());
        collect(result, schemes.getXmpDynamicMedia());
        collect(result, schemes.getXmpMediaManagement());
    }
    for (MetadataProperty p : metadata.findProperties(new NamedPropertySpec())) {
        if (!result.containsKey(p.getName())) put(result, p);
    }
}
return result;
```

The trailing sweep is the completeness guarantee: plugin and vendor packets the named schemes miss still land in the map under their qualified names.

### When is the full snapshot worth it over scheme reads?

At ingestion and audit time. The snapshot costs one file open and returns everything, which suits jobs that store metadata for later queries. Scheme reads answer a single question faster and keep request handlers lean. The break-even is simple: if your code would call two or more scheme readers on the same file, take the snapshot and share the map.

### Operation 2: Dublin Core read

**Overview:** returns the dc:* interoperability fields (Title, Creator, Description, Subject, Rights, Format, Identifier, Source, Coverage) as a map.

#### How It Works

Two early returns handle the routine sparse cases, then the loop converts each property with the interpreted-value-first pattern. Dublin Core is the layer most DAM systems, indexers, and licensing checks agree on, which makes this the default request-time read.

#### Implementation

```java
Map<String, String> result = new LinkedHashMap<>();
try (Metadata metadata = new Metadata(adobeFilePath)) {
    IXmp root = (IXmp) metadata.getRootPackage();
    if (root == null || root.getXmpPackage() == null) return result;
    XmpDublinCorePackage dc = root.getXmpPackage().getSchemes().getDublinCore();
    if (dc == null) return result;
    for (MetadataProperty p : dc) {
        String value = "";
        if (p.getInterpretedValue() != null
                && p.getInterpretedValue().getRawValue() != null) {
            value = String.valueOf(p.getInterpretedValue().getRawValue());
        } else if (p.getValue() != null && p.getValue().getRawValue() != null) {
            value = String.valueOf(p.getValue().getRawValue());
        }
        result.put(p.getName(), value);
    }
}
return result;
```

### Operation 3: Photoshop scheme read

**Overview:** returns the photoshop:* editorial and location fields that power Bridge, Lightroom, and DAM search filters.

#### How It Works

Eight typed getters cover ColorMode, IccProfile, City, Country, DateCreated, CaptionWriter, Credit, and Source. The private `safePut` helper converts nulls and getter exceptions into empty strings, so a partially populated file never breaks a batch job. An empty string means the standard defines the field but this file does not carry it.

#### Implementation

```java
Map<String, String> result = new LinkedHashMap<>();
try (Metadata metadata = new Metadata(adobeFilePath)) {
    IXmp root = (IXmp) metadata.getRootPackage();
    if (root == null || root.getXmpPackage() == null) return result;
    XmpPhotoshopPackage ps = root.getXmpPackage().getSchemes().getPhotoshop();
    if (ps == null) return result;

    safePut(result, "photoshop:ColorMode", () -> ps.getColorMode());
    safePut(result, "photoshop:IccProfile", () -> ps.getIccProfile());
    safePut(result, "photoshop:City", () -> ps.getCity());
    safePut(result, "photoshop:Country", () -> ps.getCountry());
    safePut(result, "photoshop:DateCreated", () -> ps.getDateCreated());
    safePut(result, "photoshop:CaptionWriter", () -> ps.getCaptionWriter());
    safePut(result, "photoshop:Credit", () -> ps.getCredit());
    safePut(result, "photoshop:Source", () -> ps.getSource());
}
return result;
```

### Operation 4: Copyright and creator write

**Overview:** stamps dc:rights and dc:creator into Dublin Core and mirrors the identity into xmp:CreatorTool.

#### How It Works

The guards come first: a missing packet gets a fresh `XmpPacketWrapper`, a missing scheme gets `XmpDublinCorePackage`, and only then do `setRights` and `set` with an Ordered `XmpArray` write values. Mirroring `setCreatorTool` keeps tools that read XmpBasic in agreement with Dublin Core readers. I have watched a licensing banner show "Unknown author" purely because a viewer read one scheme while the writer wrote the other; updating both ends that class of bug.

#### Implementation

```java
try (Metadata metadata = new Metadata(inputPath)) {
    IXmp root = (IXmp) metadata.getRootPackage();
    if (root == null) return;
    if (root.getXmpPackage() == null) {
        root.setXmpPackage(new XmpPacketWrapper());
    }
    if (root.getXmpPackage().getSchemes().getDublinCore() == null) {
        root.getXmpPackage().getSchemes().setDublinCore(new XmpDublinCorePackage());
    }
    XmpDublinCorePackage dc = root.getXmpPackage().getSchemes().getDublinCore();
    dc.setRights(copyright);
    dc.set("dc:creator", XmpArray.from(new String[]{creator}, XmpArrayType.Ordered));

    if (root.getXmpPackage().getSchemes().getXmpBasic() == null) {
        root.getXmpPackage().getSchemes().setXmpBasic(new XmpBasicPackage());
    }
    root.getXmpPackage().getSchemes().getXmpBasic().setCreatorTool(creator);

    metadata.save(outputPath);
}
```

### Operation 5: Keyword write

**Overview:** writes the dc:subject bag, the vocabulary DAM search indexes.

#### How It Works

Same guard-create pattern, then one `set` call with an Unordered `XmpArray`, matching how indexers treat keyword bags. The write replaces the existing list, so read and merge in Java first when tagging must be additive. `Main.java` writes three sample keywords and asserts the first appears in the saved bytes.

#### Implementation

```java
try (Metadata metadata = new Metadata(inputPath)) {
    IXmp root = (IXmp) metadata.getRootPackage();
    if (root == null) return;
    if (root.getXmpPackage() == null) {
        root.setXmpPackage(new XmpPacketWrapper());
    }
    if (root.getXmpPackage().getSchemes().getDublinCore() == null) {
        root.getXmpPackage().getSchemes().setDublinCore(new XmpDublinCorePackage());
    }
    root.getXmpPackage().getSchemes().getDublinCore().set(
            "dc:subject", XmpArray.from(keywords, XmpArrayType.Unordered));
    metadata.save(outputPath);
}
```

## Real-World Use Cases

A media house ingests agency PSD deliveries with the snapshot and stores each map beside the asset record; rights questions afterward are database lookups. A marketplace runs the Dublin Core read as its listing gate and rejects uploads without dc:rights, pointing sellers to the stamping operation. A studio retagging its archive after a taxonomy change loops the keyword write over both psd metadata and adobe illustrator metadata with the same code path.

## Common Pitfalls and How to Avoid Them

1. **Assuming the packet exists**
   - **Problem:** direct scheme access throws or misbehaves on fresh exports without XMP.
   - **Solution:** keep the guard-create pattern from both write operations; it is why batch stamping is safe.

2. **Replacing keywords when you meant to add**
   - **Problem:** `set("dc:subject", ...)` overwrites the whole bag.
   - **Solution:** read the current bag, extend the Java array, and write the merged result.

3. **Reading raw values into reports**
   - **Problem:** raw enumerations and date objects stringify into confusing output.
   - **Solution:** keep the `getInterpretedValue()`-first pattern used by every reader here.

## FAQ

**Q: Does the same code serve PSD and AI files?**
A: Yes. Both containers resolve through the `IXmp` cast, and the repository seeds one of each to prove it. Expect AI exports to populate fewer schemes, which the early returns and empty-string conventions handle without special cases.

**Q: How do I verify a write actually persisted?**
A: Re-read the output the way `Main.java` does: it asserts the copyright and creator strings survive a re-read and checks the first keyword lands in the saved bytes. A read-back after save is cheap insurance in any automated pipeline.

**Q: What about licensing?**
A: Evaluation mode runs everything in this guide. `Main.java` applies a license from `LicensePath` when the file exists and prints a warning otherwise; a [temporary license](https://purchase.groupdocs.com/temp-license/100234) lifts evaluation limits for production use.

## Conclusion

Two reads for questions, one read for everything, two writes for ownership and search. That is the complete XMP surface a Java integration needs for Adobe files, and all five operations share one resolution pattern, so learning the first makes the rest familiar. Start with the snapshot at ingestion, add the writes where your pipeline exports, and keep the read-back assertions; they are what turn a metadata script into something an audit accepts.

**Next Steps:**
- Build and run the [complete repository](https://github.com/groupdocs-metadata/read-and-write-xmp-in-psd-ai-files-java) with `mvn compile exec:java`
- Browse the [API Reference](https://reference.groupdocs.com/metadata/java/) for the full XMP surface

## See Also

- [In-depth blog article about this project](https://blog.groupdocs.com/metadata/xmp-metadata-in-adobe-psd-and-ai-java/) – the before/after story behind the repo
- [Working with XMP metadata](https://docs.groupdocs.com/metadata/java/working-with-xmp-metadata/) – reference for reading, updating, and adding XMP packages
- [Product documentation](https://docs.groupdocs.com/metadata/java/) – getting started and advanced topics
- [API Reference](https://reference.groupdocs.com/metadata/java/) – full API details for GroupDocs.Metadata for Java
