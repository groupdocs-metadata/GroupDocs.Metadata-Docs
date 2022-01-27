---
id: groupdocs-metadata-for-net-22-1-release-notes
url: metadata/net/groupdocs-metadata-for-net-22-1-release-notes
title: GroupDocs.Metadata for .NET 22.1 Release Notes
weight: 11
description: ""
keywords: 
productName: GroupDocs.Metadata for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Metadata for .NET 22.1{{< /alert >}}

## Major Features


There are the following features, enhancements, and fixes in this release:

*   Implement FileType class containing list of supported formats

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| METADATANET-3938 | Implement FileType class containing list of supported formats | Improvement         |



## Public API and Backward Incompatible Changes

### Implement FileType class containing list of supported formats

This improvement allows you to get a list of file formats supported by the library.

##### Public API changes

Added class **FileType** to namespace **GroupDocs.Metadata.Options.Common**.
Added property **string Description** to class **GroupDocs.Metadata.Common.FileType**.
Added property **string Extension** to class **GroupDocs.Metadata.Common.FileType**.
Added property **FileFormat FileFormat** to class **GroupDocs.Metadata.Common.FileType**.
Added method **IEnumerable\<FileType\> GetSupportedFileTypes()** to class **GroupDocs.Metadata.Common.FileType**.

Added field **FileType Unknown** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType TORRENT** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType PDF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType PPT** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType PPTX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType POTX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType PPTM** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType POTM** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType PPS** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType PPSX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType PPSM** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType POT** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType XLS** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType XLSX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType XLSM** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType XLT** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType XLTX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType XLTM** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType XLSB** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType ODS** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType DOC** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType DOCX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType DOT** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType DOTX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType DOCM** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType DOTM** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType ODT** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType TIF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType TIFF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType JPEG** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType JPG** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType JPE** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType PSD** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType JP2** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType J2K** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType JPF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType JPX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType JPM** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType MJ2** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType GIF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType PNG** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType BMP** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType WMF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType EMF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType DJVU** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType DJV** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType VDX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType VSD** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType VSDX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType VSS** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType VSX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType VTX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType ONE** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType MPP** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType MPT** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType WAV** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType MP3** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType AVI** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType FLV** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType ASF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType MOV** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType QT** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType MKA** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType MKV** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType MK3D** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType WEBM** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType ZIP** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType ZIPX** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType JAR** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType VCF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType VCR** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType EPUB** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType TTF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType TTC** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType OTF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType OTC** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType DXF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType DWG** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType EML** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType MSG** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType WEBP** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType DICOM** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType HEIF** to class **GroupDocs.Metadata.Common.FileType**.
Added field **FileType HEIC** to class **GroupDocs.Metadata.Common.FileType**.

##### Use cases

The following example demonstrates how to get the list of supported formats.

```csharp
IEnumerable<FileType> supportedFileTypes = FileType
    .GetSupportedFileTypes()
    .OrderBy(ft => ft.Extension);
 
foreach (FileType fileType in supportedFileTypes)
{
    Console.WriteLine(fileType.Extension.PadRight(8) + " - " + fileType.Description);
}
```
