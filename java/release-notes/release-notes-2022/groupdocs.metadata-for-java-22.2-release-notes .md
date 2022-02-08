---
id: groupdocs-metadata-for-java-22-2-release-notes
url: metadata/java/groupdocs-metadata-for-java-22-2-release-notes
title: GroupDocs.Metadata for Java 22.2 Release Notes
weight: 12
description: ""
keywords: 
productName: GroupDocs.Metadata for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Metadata for Java 22.2{{< /alert >}}

## Major Features


There are the following features, enhancements and fixes in this release:

*   Implement FileType class containing list of supported formats
*   Fix java.lang.IllegalAccessError when loading PPTX file

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| METADATANET-3938 | Implement FileType class containing list of supported formats | Improvement |
| METADATAJAVA-493 | Fix java.lang.IllegalAccessError when loading PPTX file | Bug |



## Public API and Backward Incompatible Changes

### Implement FileType class containing list of supported formats

This improvement allows you to get a list of file formats supported by the library.

##### Public API changes

Added class **FileType** to package **com.groupdocs.metadata.core**.
Added method **String getDescription()** to class **com.groupdocs.metadata.core.FileType**.
Added method **String getExtension()** to class **com.groupdocs.metadata.core.FileType**.
Added method **FileFormat getFileFormat()** to class **com.groupdocs.metadata.core.FileType**.
Added method **Iterable\<FileType\> getSupportedFileTypes()** to class **com.groupdocs.metadata.core.FileType**.

Added field **FileType Unknown** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType TORRENT** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType PDF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType PPT** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType PPTX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType POTX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType PPTM** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType POTM** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType PPS** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType PPSX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType PPSM** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType POT** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType XLS** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType XLSX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType XLSM** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType XLT** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType XLTX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType XLTM** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType XLSB** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType ODS** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType DOC** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType DOCX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType DOT** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType DOTX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType DOCM** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType DOTM** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType ODT** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType TIF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType TIFF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType JPEG** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType JPG** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType JPE** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType PSD** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType JP2** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType J2K** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType JPF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType JPX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType JPM** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType MJ2** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType GIF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType PNG** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType BMP** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType WMF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType EMF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType DJVU** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType DJV** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType VDX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType VSD** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType VSDX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType VSS** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType VSX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType VTX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType ONE** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType MPP** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType MPT** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType WAV** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType MP3** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType AVI** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType FLV** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType ASF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType MOV** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType QT** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType MKA** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType MKV** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType MK3D** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType WEBM** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType ZIP** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType ZIPX** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType JAR** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType VCF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType VCR** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType EPUB** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType TTF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType TTC** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType OTF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType OTC** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType DXF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType DWG** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType EML** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType MSG** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType WEBP** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType DICOM** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType HEIF** to class **com.groupdocs.metadata.core.FileType**.
Added field **FileType HEIC** to class **com.groupdocs.metadata.core.FileType**.

##### Use cases

The following example demonstrates how to get the list of supported formats.

```java
Iterable<FileType> supportedFileTypes = FileType.getSupportedFileTypes();
Iterator iterator = supportedFileTypes.iterator();      
while (iterator.hasNext()) {
    FileType fileType = (FileType)iterator.next();
    System.out.println(fileType.getExtension() + " - " + fileType.getDescription());
}
```

### Fix java.lang.IllegalAccessError when loading PPTX file

This fixes java.lang.IllegalAccessError when loading PPTX file.

##### Public API changes

None.

##### Use cases

None.
