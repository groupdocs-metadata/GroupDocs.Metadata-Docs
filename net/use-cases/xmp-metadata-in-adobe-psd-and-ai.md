---
id: xmp-metadata-in-adobe-psd-and-ai
url: /metadata/net/use-cases/xmp-metadata-in-adobe-psd-and-ai/
title: XMP Metadata Operations for PSD and AI Files - Technical Deep Dive
weight: 1
description: "Technical analysis of five XMP metadata operations for Photoshop PSD and Illustrator AI files in .NET: full-packet snapshot, scheme-scoped reads, and guarded ownership and keyword writes."
keywords: xmp metadata, psd metadata, adobe illustrator metadata, dublin core, photoshop scheme, dc:subject, dc:rights, groupdocs metadata net
productName: GroupDocs.Metadata for .NET
structuredData:
    showOrganization: True
toc: true
draft: false
---

{{< alert style="info" >}}
💡 Full working example available on GitHub:
[edit-xmp-in-psd-and-ai-files-using-groupdocs-metadata-dotnet](https://github.com/groupdocs-metadata/edit-xmp-in-psd-and-ai-files-using-groupdocs-metadata-dotnet)
{{< /alert >}}

## Executive Summary

XMP editing is a GroupDocs.Metadata capability for .NET that reads and writes the metadata packet embedded in Photoshop PSD and Adobe Illustrator AI files. The engineering problem is not any single field; it is that xmp metadata is an XML packet inside two different binary containers, split across schemes with separate vocabularies, and production systems need it read at ingestion, checked at licensing gates, and written at export. This guide dissects the five operations a working pipeline needs, drawn from a runnable repository that asserts each one: a full-packet snapshot, two scheme-scoped reads, an ownership write that touches three property layers, and a keyword write for search. Each operation is examined for its integration shape, its cost profile, and its failure behavior on files that carry no XMP at all.

{{< alert style="warning" >}}
**For Production Use:** run the [complete implementation](https://github.com/groupdocs-metadata/edit-xmp-in-psd-and-ai-files-using-groupdocs-metadata-dotnet) against samples from your own asset pipeline before deployment.
{{< /alert >}}

## What This Guide Covers

You will see how one `IXmp` cast serves both PSD and AI containers, why the write operations guard-create missing schemes, and how the dual identity write keeps XMP-aware and XMP-blind readers consistent. The audience is .NET engineers integrating Adobe assets into DAM, licensing, or search systems, and architects deciding where metadata work belongs in an asset pipeline.

**Prerequisites:**
- .NET SDK 8.0 (the repository targets `net8.0`)
- GroupDocs.Metadata 26.6.0 from NuGet

## Operation Comparison Matrix

| Operation | Direction | Scope | Cost Profile | Failure Mode on XMP-less Files |
|-----------|-----------|-------|-------------|-------------------------------|
| **Full snapshot** | Read | Everything | One open, whole tree walk | Empty dictionary |
| **Dublin Core read** | Read | dc:* only | One open, one scheme | Empty dictionary |
| **Photoshop read** | Read | photoshop:* only | One open, eight getters | Empty dictionary |
| **Ownership write** | Write | Three layers | One open, one save | Creates packet and schemes |
| **Keyword write** | Write | dc:subject | One open, one save | Creates packet and scheme |

## Operation 1: Full-Packet Snapshot

The foundation read. Casting the root package to `IXmp` exposes the packet; the method then iterates it, walks seven named schemes through a small `CollectScheme` helper, and finishes with a deep sweep.

```csharp
var result = new Dictionary<string, string>();
using (var metadata = new MetadataFacade(adobeFilePath))
{
    var root = metadata.GetRootPackage() as IXmp;
    if (root?.XmpPackage == null) return result;

    foreach (var property in root.XmpPackage)
    {
        var value = property.InterpretedValue?.ToString()
            ?? property.Value?.ToString() ?? string.Empty;
        result[property.Name] = value;
    }

    var schemes = root.XmpPackage.Schemes;
    CollectScheme(schemes.DublinCore, result);
    CollectScheme(schemes.XmpBasic, result);
    CollectScheme(schemes.Photoshop, result);
    CollectScheme(schemes.CameraRaw, result);
    CollectScheme(schemes.PagedText, result);
    CollectScheme(schemes.XmpDynamicMedia, result);
    CollectScheme(schemes.XmpMediaManagement, result);
```

```csharp
    var deepProps = metadata.FindProperties(p => p.Name != null);
    foreach (var p in deepProps)
    {
        var value = p.InterpretedValue?.ToString()
            ?? p.Value?.ToString() ?? string.Empty;
        if (!result.ContainsKey(p.Name)) result[p.Name] = value;
    }
}
return result;
```

Integration shape: run it once at ingestion, store the dictionary, and let every later question hit your database instead of the file. `InterpretedValue` first means dates and enumerations arrive readable. One file open covers the entire operation, which is the property that makes bulk ingestion tractable. I benchmarked nothing here on purpose; the honest statement is that the file open dominates each operation, and batching is where the wins live.

## Can I trust the snapshot to include vendor-specific packets?

Yes, because the snapshot ends with a FindProperties sweep over the whole tree. The seven named schemes cover standard Adobe vocabularies, and the final pass adds any property the schemes missed, keyed by qualified name. Custom packets written by plugins or asset tools surface in the same dictionary, so downstream indexing code never needs a special case.

## Operation 2: Dublin Core Read

The interoperability read. Title, Creator, Description, Subject, Rights, Format, Identifier, Source, and Coverage are the dc:* fields most DAM systems, search indexes, and licensing checks agree on.

```csharp
var result = new Dictionary<string, string>();
using (var metadata = new MetadataFacade(adobeFilePath))
{
    var root = metadata.GetRootPackage() as IXmp;
    var dc = root?.XmpPackage?.Schemes?.DublinCore;
    if (dc == null) return result;

    foreach (var property in dc)
    {
        var key = property.Name;
        var value = property.InterpretedValue?.ToString()
            ?? property.Value?.ToString() ?? string.Empty;
        result[key] = value;
    }
}
return result;
```

The null-conditional chain is the operational detail: files without XMP and files whose packet lacks the scheme both produce an empty dictionary. In request handlers, prefer this over the snapshot; nine fields read faster than a whole tree.

## Operation 3: Photoshop Scheme Read

The editorial-context read. ColorMode and IccProfile answer print-workflow questions; City, Country, DateCreated, CaptionWriter, Credit, and Source drive the filters Bridge, Lightroom, and DAM search expose for psd metadata.

```csharp
var result = new Dictionary<string, string>();
using (var metadata = new MetadataFacade(adobeFilePath))
{
    var root = metadata.GetRootPackage() as IXmp;
    var ps = root?.XmpPackage?.Schemes?.Photoshop;
    if (ps == null) return result;

    result["photoshop:ColorMode"] = ps.ColorMode?.ToString() ?? string.Empty;
    result["photoshop:IccProfile"] = ps.IccProfile ?? string.Empty;
    result["photoshop:City"] = ps.City ?? string.Empty;
    result["photoshop:Country"] = ps.Country ?? string.Empty;
    result["photoshop:DateCreated"] = ps.DateCreated?.ToString() ?? string.Empty;
    result["photoshop:CaptionWriter"] = ps.CaptionWriter ?? string.Empty;
    result["photoshop:Credit"] = ps.Credit ?? string.Empty;
    result["photoshop:Source"] = ps.Source ?? string.Empty;
}
return result;
```

Empty strings mean the field exists in the standard but not in this file, a distinction that matters when adobe illustrator metadata arrives sparse from fresh exports.

## Operation 4: Ownership Write

The compliance write. It touches three property layers so every reader, XMP-aware or not, sees the same identity afterward.

```csharp
using (var metadata = new MetadataFacade(inputPath))
{
    var root = metadata.GetRootPackage() as IXmp;
    if (root == null) return;

    if (root.XmpPackage == null)
    {
        root.XmpPackage = new XmpPacketWrapper();
    }
    if (root.XmpPackage.Schemes.DublinCore == null)
    {
        root.XmpPackage.Schemes.DublinCore = new XmpDublinCorePackage();
    }

    var dc = root.XmpPackage.Schemes.DublinCore;
    dc.SetRights(copyright);
    dc.Set("dc:creator", XmpArray.From(new[] { creator }, XmpArrayType.Ordered));
```

```csharp
    if (root.XmpPackage.Schemes.XmpBasic == null)
    {
        root.XmpPackage.Schemes.XmpBasic = new XmpBasicPackage();
    }
    root.XmpPackage.Schemes.XmpBasic.CreatorTool = creator;

    metadata.SetProperties(p => p.Tags.Contains(Tags.Person.Creator),
        new PropertyValue(creator));

    metadata.Save(outputPath);
}
```

Design note: the `SetProperties` call with `Tags.Person.Creator` is what elevates this from an XMP write to an identity write. Properties the library classifies as creator fields get the value wherever the format stores them. The guard-create pattern above it makes the whole method safe for batch runs over mixed archives. Saving to `outputPath` preserves originals, and the repository's `Program.cs` asserts both written strings survive a re-read.

## Operation 5: Keyword Write

The search write. dc:subject is the tag vocabulary DAM indexers read, written here as one unordered array.

```csharp
using (var metadata = new MetadataFacade(inputPath))
{
    var root = metadata.GetRootPackage() as IXmp;
    if (root == null) return;

    if (root.XmpPackage == null)
    {
        root.XmpPackage = new XmpPacketWrapper();
    }
    if (root.XmpPackage.Schemes.DublinCore == null)
    {
        root.XmpPackage.Schemes.DublinCore = new XmpDublinCorePackage();
    }

    root.XmpPackage.Schemes.DublinCore.Set(
        "dc:subject",
        XmpArray.From(keywords, XmpArrayType.Unordered));
    metadata.Save(outputPath);
}
```

`XmpArrayType.Unordered` matches indexer semantics: keyword order carries no meaning. The write replaces the previous bag, so additive tagging means read, merge in C#, write.

## Operational Considerations

Error behavior is deliberately soft on the read side; empty dictionaries are data, not faults, and pipelines should treat them as "no metadata present". On the write side, always target a separate output path in automated runs, and keep a read-back assertion in the pipeline the way `Program.cs` does, because container-level surprises are cheaper to catch at write time than at delivery time.

Threading and batching follow from the cost model. Each operation is one `Metadata` instance over one file, and instances are independent, so parallelizing across files is the natural scale-out; parallelizing within a file buys nothing. Cache the ingestion snapshot per asset and let gates read from the cache when the file has not changed, which cuts most gate traffic to zero file opens.

Licensing follows the usual model: evaluation mode reproduces everything in this guide, and a [temporary license](https://purchase.groupdocs.com/temp-license/100216) lifts evaluation limits for production batches.

## FAQ

**Q: Do these operations behave identically on AI files?**
A: Yes. Both containers resolve through the same `IXmp` cast, and the repository seeds `sample.ai` alongside `sample.psd` to prove it. The practical difference is population: fresh Illustrator exports typically carry fewer filled schemes, which the guards and empty-string conventions absorb.

**Q: What happens to other schemes when I write Dublin Core?**
A: They are preserved. The write methods modify only the schemes they touch, and the underlying PSD or AI layer data is untouched by design, which the method's remarks call out for batch processing.

**Q: How does this relate to EXIF and IPTC in the same files?**
A: XMP is one of three metadata standards a PSD can carry. The same library reads EXIF and IPTC through their own packages, so a complete asset profile usually combines the XMP snapshot here with those reads. The [Working with XMP metadata](https://docs.groupdocs.com/metadata/net/working-with-xmp-metadata/) page covers the XMP side in reference form.

## Conclusion

Five operations cover the production surface: snapshot at ingestion, scoped reads at request time, and two guarded writes at export. The pattern underneath is constant, one `IXmp` cast, one scheme access, one typed read or write, and the repository proves the whole set on both containers. Start with the snapshot; every later feature reuses its output.

**Next Steps:**
- Run the [complete source code](https://github.com/groupdocs-metadata/edit-xmp-in-psd-and-ai-files-using-groupdocs-metadata-dotnet) against your own PSD and AI samples
- Review the [API Reference](https://reference.groupdocs.com/metadata/net/) for the full XMP surface

## See Also

- [In-depth blog article about this project](https://blog.groupdocs.com/metadata/xmp-metadata-in-adobe-psd-and-ai-net/) – business context and the full pipeline
- [Working with XMP metadata](https://docs.groupdocs.com/metadata/net/working-with-xmp-metadata/) – reference for reading, updating, and adding XMP packages
- [Product documentation](https://docs.groupdocs.com/metadata/net/) – getting started and advanced topics
- [API Reference](https://reference.groupdocs.com/metadata/net/) – full API details for GroupDocs.Metadata for .NET
