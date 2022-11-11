---
id: groupdocs-metadata-for-java-22-11-release-notes
url: metadata/java/groupdocs-metadata-for-java-22-11-release-notes
title: GroupDocs.Metadata for Java 22.11 Release Notes
weight: 10
description: ""
keywords: 
productName: GroupDocs.Metadata for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Metadata for Java 22.11{{< /alert >}}

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

The [Tags.Content.Body](https://apireference-qa.groupdocs.com/metadata/java/com.groupdocs.metadata.tagging/ContentTagCategory#getBody--) property has been added to the [ContentTagCategory](https://apireference-qa.groupdocs.com/metadata/java/com.groupdocs.metadata.tagging/ContentTagCategory) namespace

The [Tags.Person.Recipient](https://apireference-qa.groupdocs.com/metadata/java/com.groupdocs.metadata.tagging/PersonTagCategory#getRecipient--) property has been added to the [PersonTagCategory](https://apireference-qa.groupdocs.com/metadata/java/com.groupdocs.metadata.tagging/PersonTagCategory) namespace

The [Tags.Person.Artist](https://apireference-qa.groupdocs.com/metadata/java/com.groupdocs.metadata.tagging/PersonTagCategory#getArtist--) property has been added to the [PersonTagCategory](https://apireference-qa.groupdocs.com/metadata/java/com.groupdocs.metadata.tagging/PersonTagCategory) namespace
