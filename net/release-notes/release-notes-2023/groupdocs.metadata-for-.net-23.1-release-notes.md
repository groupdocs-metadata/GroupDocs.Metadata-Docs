---
id: groupdocs-metadata-for-net-23-1-release-notes
url: metadata/net/groupdocs-metadata-for-net-23-1-release-notes
title: GroupDocs.Metadata for .NET 23.1 Release Notes
weight: 5
description: ""
keywords: 
productName: GroupDocs.Metadata for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Metadata for .NET 23.1{{< /alert >}}

## Major Features


There are the following features, enhancements, and fixes in this release:

*   Add support for .dng RAW format

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| METADATANET-3989 | Support DNG format. | New Feature         |

## Public API and Backward Incompatible Changes

### Implement the ability to configure cache for heavy operations

This improvement allows you to extract metadata from the .dng format

##### Public API changes

The [DngRootPackage](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.formats.image/dngrootpackage)
class has been added to
the [GroupDocs.Metadata.Formats.Image](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.formats.image)
namespace

The Dng item has been added to the
[FileFormat](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.common/fileformat)
enum
##### Use cases

Read XMP metadata properties from a HEIC image


```csharp
using (Metadata metadata = new Metadata(@"D:\sample.dng"))
{
	var root = metadata.GetRootPackage<DngRootPackage>();

	Console.WriteLine(root.DngPackage.Artist);
	Console.WriteLine(root.DngPackage.Description);
}
```