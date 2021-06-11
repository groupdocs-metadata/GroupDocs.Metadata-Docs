---
id: groupdocs-metadata-for-java-21-6-release-notes
url: metadata/java/groupdocs-metadata-for-java-21-6-release-notes
title: GroupDocs.Metadata for Java 21.6 Release Notes
weight: 8
description: ""
keywords: 
productName: GroupDocs.Metadata for Java
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Metadata for Java 21.6{{< /alert >}}

## Major Features

  
There are the following features, enhancements and fixes in this release:

*   Implement property interpreters for all enum types across all formats

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| METADATANET-3846 | Implement property interpreters for all enum types across all formats                               | Improvement         |


## Public API and Backward Incompatible Changes

### Implement property interpreters for all enum types across all formats

This improvement allows the user to get a user-friendly interpretation of a metadata property representing an enum value. Please refer to [this article]({{< ref "metadata/java/developer-guide/advanced-usage/working-with-interpreted-values.md" >}}) for more information.

##### Public API changes

None

##### Use cases 

Obtain a full list of properties that provide an interpreted value

```java
public class WorkingWithInterpretedValues {
     public static void run() {
        File folder = new File(Constants.InputPath);
        for (File file : folder.listFiles()) {
            try (Metadata metadata = new Metadata(file.getAbsolutePath())) {
                if (metadata.getFileFormat() != FileFormat.Unknown && !metadata.getDocumentInfo().isEncrypted()) {
                    System.out.println();
                    System.out.println(file.getName());
                      
                    IReadOnlyList<MetadataProperty> properties = metadata.findProperties(
                            new WorkingWithInterpretedValues().new InterpretedValueIsNotNullSpecification());
                    for (MetadataProperty property : properties) {
                            System.out.println(property.getName());
                            System.out.println(property.getValue().getRawValue());
                            System.out.println(property.getInterpretedValue().getRawValue());
                    }
                }
            }
        }
    }
       
    private class InterpretedValueIsNotNullSpecification extends Specification {
        public /*override*/ boolean isSatisfiedBy(MetadataProperty candidate) {
            return candidate.getInterpretedValue() != null;
        }
    }
}
```