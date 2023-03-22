---
id: groupdocs-metadata-for-net-23-3-release-notes
url: metadata/net/groupdocs-metadata-for-net-23-3-release-notes
title: GroupDocs.Metadata for .NET 23.3 Release Notes
weight: 4
description: ""
keywords: 
productName: GroupDocs.Metadata for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Metadata for .NET 23.3{{< /alert >}}

## Major Features


There are the following features, enhancements, and fixes in this release:

*   Add support for .cr RAW format

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| METADATANET-3990 | Support CR2 format. | New Feature         |

## Public API and Backward Incompatible Changes

### Implement the ability to configure cache for heavy operations

This improvement allows you to extract metadata from the .cr2 format

##### Public API changes

The [Cr2RootPackage](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.formats.raw/cr2rootpackage)
class has been added to
the [GroupDocs.Metadata.Formats.Raw](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.formats.raw)
namespace

The Cr2 item has been added to the
[FileFormat](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.common/fileformat)
enum

The [GpsIfdPackage](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.formats.raw/gpsifdpackage)
class has been added to
the [GroupDocs.Metadata.Formats.Raw](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.formats.raw)
namespace

The [InteroperabilityIFDPointerPackage](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.formats.raw/interoperabilityifdpointerpackage)
class has been added to
the [GroupDocs.Metadata.Formats.Raw](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.formats.raw)
namespace

And many makernote tags and indexes added to [GroupDocs.Metadata.Formats.Raw.Cr2](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.formats.raw.cr2)
##### Use cases

Read metadata properties


```csharp
using (Metadata metadata = new Metadata(@"D:\sample.cr2"))
{
	var root = metadata.GetRootPackage<Cr2RootPackage>();

	Console.WriteLine(root.Cr2Package.RawTiffTagPackage.Artist);
    Console.WriteLine(root.Cr2Package.RawTiffTagPackage.Copyright);
}
```

Write metadata properties


```csharp
using (Metadata metadata = new Metadata(@"D:\sample.cr2"))
{
	Cr2RootPackage root = metadata.GetRootPackage<Cr2RootPackage>();
	RawTiffTagPackage newRawTiffTagPackage;
	newRawTiffTagPackage = (RawTiffTagPackage)root.Cr2Package.RawTiffTagPackage ?? new RawTiffTagPackage();
	newRawTiffTagPackage.Model = "New model name";
	root.Cr2Package.RawTiffTagPackage = newRawTiffTagPackage;
	root.Save(outPath);
}
```

Get interpreted value


```csharp
using (Metadata metadata = new Metadata(@"D:\sample.cr2"))
{
	var root = metadata.GetRootPackage<Cr2RootPackage>();
	Cr2MakerNotePackage cr2MakerNotePackage = (Cr2MakerNotePackage) root.Cr2Package.RawTiffTagPackage.RawExifTagPackage.RawMakerNotePackage;
	Console.WriteLine(cr2MakerNotePackage.Cr2CameraSettingsPackage.MacroMode);
	var propertyMacroMode = cr2MakerNotePackage.Cr2CameraSettingsPackage[(uint)Cr2CameraSettingsIndex.MacroMode] as RawShortTag;
	Console.WriteLine(propertyMacroMode.InterpretedValue);
}
```