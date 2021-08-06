---
id: groupdocs-metadata-for-net-21-8-release-notes
url: metadata/net/groupdocs-metadata-for-net-21-8-release-notes
title: GroupDocs.Metadata for .NET 21.8 Release Notes
weight: 6
description: ""
keywords: 
productName: GroupDocs.Metadata for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Metadata for .NET 21.8{{< /alert >}}

## Major Features


There are the following features, enhancements, and fixes in this release:

*   Implement the ability to configure cache for heavy operations

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| METADATANET-3891 | Implement the ability to configure cache for heavy operations | Improvement         |



## Public API and Backward Incompatible Changes

### Implement the ability to configure cache for heavy operations

This improvement allows you to specify the path to the cache folder for processing large images, the maximum cache size on disk, and the maximum cache size in memory.

##### Public API changes

Added property **string CacheFolder** to class **GroupDocs.Metadata.Options.PreviewOptions**.
Added property **int MaxDiskSpaceForCache** to class **GroupDocs.Metadata.Options.PreviewOptions**.
Added property **int MaxMemoryForCache** to class **GroupDocs.Metadata.Options.PreviewOptions**.

##### Use cases

The following example demonstrates how to set cache options in an application.

```csharp
string filePath = @"C:\Documents\MyDocument.pdf";
string cachePath = @"C:\Temp";
  
using (Metadata metadata = new Metadata(filePath))
{
    PreviewOptions previewOptions = new PreviewOptions(
        pageNumber =>
        {
            string path = filePath + "_" + pageNumber + ".jpg";
            Stream stream = File.Create(path);
            return stream;
        },
        (pageNumber, pageStream) =>
        {
            pageStream.Close();
        })
    {
        CacheFolder = cachePath,
        PreviewFormat = PreviewOptions.PreviewFormats.JPEG,
        PageNumbers = new int[] { 1 },
        Width = 100,
        Height = 100
    };
  
    metadata.GeneratePreview(previewOptions);
}
```