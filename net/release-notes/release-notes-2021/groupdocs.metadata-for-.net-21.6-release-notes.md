---
id: groupdocs-metadata-for-net-21-6-release-notes
url: metadata/net/groupdocs-metadata-for-net-21-6-release-notes
title: GroupDocs.Metadata for .NET 21.6 Release Notes
weight: 7
description: ""
keywords: 
productName: GroupDocs.Metadata for .NET
hideChildren: False
---
{{< alert style="info" >}}This page contains release notes for GroupDocs.Metadata for .NET 21.6{{< /alert >}}

## Major Features

  
There are the following features, enhancements and fixes in this release:

*   Implement property interpreters for all enum types across all formats

## Full List of Issues Covering all Changes in this Release

| Key | Summary | Category |
| --- | --- | --- |
| METADATANET-3846 | Implement property interpreters for all enum types across all formats                               | Improvement         |



## Public API and Backward Incompatible Changes

### Implement property interpreters for all enum types across all formats

This improvement allows the user to get a user-friendly interpretation of a metadata property representing an enum value. Please refer to [this article]({{< ref "metadata/net/developer-guide/advanced-usage/working-with-interpreted-values.md" >}}) for more information.

##### Public API changes 

None

##### Use cases 

Obtain a full list of properties that provide an interpreted value

```csharp
foreach (string file in Directory.GetFiles(Constants.InputPath))
{
    using (Metadata metadata = new Metadata(file))
    {
        if (metadata.FileFormat != FileFormat.Unknown && !metadata.GetDocumentInfo().IsEncrypted)
        {
            Console.WriteLine();
            Console.WriteLine(file);
            var properties = metadata.FindProperties(p => p.InterpretedValue != null);
            foreach (var property in properties)
            {
                Console.WriteLine(property.Name);
                Console.WriteLine(property.Value.RawValue);
                Console.WriteLine(property.InterpretedValue.RawValue);
            }
        }
    }
}
```