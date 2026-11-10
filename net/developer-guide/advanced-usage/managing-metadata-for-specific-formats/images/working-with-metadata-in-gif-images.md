---
id: working-with-metadata-in-gif-images
url: metadata/net/working-with-metadata-in-gif-images
title: Working with metadata in GIF images
weight: 4
description: ""
keywords: 
productName: GroupDocs.Metadata for .NET
hideChildren: False
---
## Detecting the GIF version

The following sample of code will help you to detect the version of a loaded GIF image and extract some additional file format information.

1.  [Load]({{< ref "metadata/net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a GIF image
2.  Extract the root metadata package
3.  Use the [FileType](https://reference.groupdocs.com/net/metadata/groupdocs.metadata.formats.image/gifrootpackage/properties/filetype) property to obtain file format information

**AdvancedUsage.ManagingMetadataForSpecificFormats.<WBR>Image.Gif.GifReadFileFormatProperties**

```csharp
using (Metadata metadata = new Metadata(Constants.InputGif))
{
	var root = metadata.GetRootPackage<GifRootPackage>();

	Console.WriteLine(root.FileType.FileFormat);
	Console.WriteLine(root.FileType.Version);
	Console.WriteLine(root.FileType.ByteOrder);
	Console.WriteLine(root.FileType.MimeType);
	Console.WriteLine(root.FileType.Extension);
	Console.WriteLine(root.FileType.Width);
	Console.WriteLine(root.FileType.Height);
}
```

## Working with XMP Metadata

GroupDocs.Metadata for .NET also allows managing XMP metadata in GIF files. For more details please refer to the following guide: [Working with XMP metadata]({{< ref "metadata/net/developer-guide/advanced-usage/working-with-metadata-standards/working-with-xmp-metadata.md" >}}).

## More resources
### GitHub examples
You may easily run the code above and see the feature in action in our GitHub examples:
*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)    

### GIF metadata management demo App
The [GIF Metadata Editing](https://products.groupdocs.app/metadata/gif) app is available to view and edit metadata of GIF images, including XMP properties and image details.
