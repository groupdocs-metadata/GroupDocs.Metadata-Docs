---
id: groupdocs-metadata-for-net-22-11-release-notes
url: metadata/net/groupdocs-metadata-for-net-22-11-release-notes
title: GroupDocs.Metadata for .NET 22.11 Release Notes
weight: 4
description: ""
keywords: 
productName: GroupDocs.Metadata for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Metadata for .NET 22.11{{< /alert >}}

## Major Features


There are the following features, enhancements, and fixes in this release:

*   Add new tags: Tags.Content.Body, Tags.Person.Recipient, Tags.Person.Artist
*   Added new mapping for old tags

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| METADATANET-3978 | Add new Tags for Metadata.NET. | Feature         |

## Public API and Backward Incompatible Changes

### Implement the ability work with new tags

This new feature allows the user work with new tags and new mapping for old tags

##### Public API changes 

The [Tags.Content.Body](https://reference.groupdocs.com/metadata/net/groupdocs.metadata.tagging/contenttagcategory/body/) property has been added to the [ContentTagCategory](https://reference.groupdocs.com/metadata/net/groupdocs.metadata.tagging/contenttagcategory) namespace

The [Tags.Person.Recipient](https://reference.groupdocs.com/metadata/net/groupdocs.metadata.tagging/persontagcategory/recipient/) property has been added to the [PersonTagCategory](https://reference.groupdocs.com/metadata/net/groupdocs.metadata.tagging/persontagcategory/) namespace

The [Tags.Person.Artist](https://reference.groupdocs.com/metadata/net/groupdocs.metadata.tagging/persontagcategory/artist/) property has been added to the [PersonTagCategory](https://reference.groupdocs.com/metadata/net/groupdocs.metadata.tagging/persontagcategory/) namespace