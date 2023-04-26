---
id: groupdocs-metadata-for-net-23-4-release-notes
url: metadata/net/groupdocs-metadata-for-net-23-4-release-notes
title: GroupDocs.Metadata for .NET 23.4 Release Notes
weight: 3
description: ""
keywords: 
productName: GroupDocs.Metadata for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Metadata for .NET 23.4{{< /alert >}}

## Major Features


There are the following features, enhancements, and fixes in this release:

*   Added ability to remove GPS tags in CR2 files

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| METADATANET-3999 | Remove GPS tags in CR2. | New Feature         |

## Public API and Backward Incompatible Changes

### Implement the ability to configure cache for heavy operations

This improvement allows you to extract metadata from the .cr2 format

##### Public API changes

The [RawIFD2Package](https://reference.groupdocs.com/metadata/net/groupdocs.metadata.formats.raw/rawifd2package/)
class has been added to
the [GroupDocs.Metadata.Formats.Raw](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.formats.raw)
namespace

The [RawIFD3Package](https://reference.groupdocs.com/metadata/net/groupdocs.metadata.formats.raw/rawifd3package/)
class has been added to
the [GroupDocs.Metadata.Formats.Raw](https://apireference.groupdocs.com/metadata/net/groupdocs.metadata.formats.raw)
namespace
##### Use cases

Remove GPS tag


```csharp
using (Metadata metadata = new Metadata(@"D:\sample.cr2"))
{
	var root = metadata.GetRootPackage<Cr2RootPackage>();

	root.Cr2Package.RawTiffTagPackage.GpsIfdPackage = null;
	root.Save(outPath);
}
```
