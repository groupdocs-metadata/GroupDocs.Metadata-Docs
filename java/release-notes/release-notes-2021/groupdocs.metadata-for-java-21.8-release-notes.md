---
id: groupdocs-metadata-for-java-21-8-release-notes
url: metadata/java/groupdocs-metadata-for-java-21-8-release-notes
title: GroupDocs.Metadata for Java 21.8 Release Notes
weight: 7
description: ""
keywords: 
productName: GroupDocs.Metadata for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Metadata for Java 21.8{{< /alert >}}

## Major Features


There are the following features, enhancements, and fixes in this release:

*   Implement the ability to configure cache for heavy operations
*   Replace constants with enum values in Java public API

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| METADATANET-3891 | Implement the ability to configure cache for heavy operations | Improvement |
| METADATANET-3918 | Replace constants with enum values in Java public API | Improvement|

## Public API and Backward Incompatible Changes

### Implement the ability to configure cache for heavy operations

This improvement allows you to specify the path to the cache folder for processing large images, the maximum cache size on disk, and the maximum cache size in memory.

##### Public API changes

Added method **String getCacheFolder()** to class **com.groupdocs.metadata.options.PreviewOptions**.
Added method **void setCacheFolder(String value)** to class **com.groupdocs.metadata.options.PreviewOptions**.
Added method **int getMaxDiskSpaceForCache()** to class **com.groupdocs.metadata.options.PreviewOptions**.
Added method **void setMaxDiskSpaceForCache(int value)** to class **com.groupdocs.metadata.options.PreviewOptions**.
Added method **int getMaxMemoryForCache()** to class **com.groupdocs.metadata.options.PreviewOptions**.
Added method **void setMaxMemoryForCache(int value)** to class **com.groupdocs.metadata.options.PreviewOptions**.

##### Use cases 

The following example demonstrates how to set cache options in an application.

```java
final String filePath = "C:\\Documents\\MyDocument.pdf";
final String cachePath = "C:\\Temp";
 
Metadata metadata = new Metadata(filePath);
 
PreviewOptions previewOptions = new PreviewOptions(
    new ICreatePageStream() {
        @Override
        public OutputStream createPageStream(int pageNumber) {
            try {
                String path = filePath + "_" + pageNumber + ".jpg";
                FileOutputStream stream = new FileOutputStream(path);    
                return stream;
            } catch(IOException e) {
                System.out.println(e);
            }
            return null;
        }
    },
    new IReleasePageStream() {
        @Override
        public void releasePageStream(int pageNumber, OutputStream pageStream) {
            try {
                pageStream.close();
            } catch (IOException e) {
                System.out.println(e);
            }
        }
    });
previewOptions.setCacheFolder(cachePath);
previewOptions.setPreviewFormat(PreviewOptions.PreviewFormats.JPEG);
previewOptions.setPageNumbers(new int[] { 1 });
previewOptions.setWidth(100);
previewOptions.setHeight(100);
 
metadata.generatePreview(previewOptions);
 
metadata.close();
```

### Replace constants with enum values in Java public API

This improvement replaces classes containing integer constants with enumerations.

##### Public API changes
Replaced all classes containing integer constants with enumerations.

##### Use cases

Previous version:
```java
ID3V1Tag tag = ...
int genre = tag.getGenreValue();
```

New version:
```java
ID3V1Tag tag = ...
ID3V1Genre genre = tag.getGenreValue();
```

{{< alert style="danger" >}}There is a breaking change in the version 21.8.{{< /alert >}}

