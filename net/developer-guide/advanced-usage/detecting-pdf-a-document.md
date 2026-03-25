---
id: detecting-pdf-a-document
url: metadata/net/detecting-pdf-a-document
title: Detecting PDF/A document
weight: 8
description: "This article shows how to detect whether a PDF file is PDF/A compliant and determine its conformance level in C# .NET solution programmatically with GroupDocs.Metadata for .NET"
keywords: C# .NET PDF PDF/A detect conformance archiving ISO compliance Metadata
productName: GroupDocs.Metadata for .NET
draft:True
hideChildren: False
---
PDF/A is an ISO-standardized version of PDF specialized for digital preservation and long-term archiving of electronic documents. Unlike standard PDF, PDF/A prohibits features unsuitable for long-term archiving, such as font linking and encryption, and requires that certain features be included, such as embedded fonts and color profiles.

There are several conformance levels defined across multiple PDF/A versions:

*   **PDF/A-1** (ISO 19005-1) — based on PDF 1.4, with conformance levels **a** (accessible) and **b** (basic)
*   **PDF/A-2** (ISO 19005-2) — based on PDF 1.7, adding conformance level **u** (Unicode)
*   **PDF/A-3** (ISO 19005-3) — same as PDF/A-2 but allows embedding arbitrary file formats as attachments

Using GroupDocs.Metadata for .NET you can quickly detect whether a loaded PDF document conforms to a PDF/A standard and determine its exact version and conformance level.

## Detecting PDF/A conformance

The following steps and C# code sample below show **how to detect PDF/A type of a PDF document in a .NET solution**:

1.  [Load]({{< ref "metadata/net/developer-guide/advanced-usage/loading-files/_index.md" >}}) a PDF document
2.  Extract the root metadata package
3.  Read the [PdfAVersion](https://reference.groupdocs.com/net/metadata/groupdocs.metadata.formats.document/pdftypepackage/properties/pdfaversion) property from the [FileType](https://reference.groupdocs.com/net/metadata/groupdocs.metadata.formats.document/pdfrootpackage/properties/filetype) package

**AdvancedUsage.DetectingPdfADocument**

```csharp
using (Metadata metadata = new Metadata(Constants.InputPdf))
{
	var root = metadata.GetRootPackage<PdfRootPackage>();

	if (root.FileType.PdfAVersion != PdfAVersion.None)
	{
		Console.WriteLine("The document is PDF/A compliant.");
		Console.WriteLine("PDF/A version: {0}", root.FileType.PdfAVersion);
	}
	else
	{
		Console.WriteLine("The document is not PDF/A compliant.");
	}
}
```

The [PdfAVersion](https://reference.groupdocs.com/net/metadata/groupdocs.metadata.formats.document/pdftypepackage/properties/pdfaversion) property returns one of the predefined [PdfAVersion](https://reference.groupdocs.com/net/metadata/groupdocs.metadata.formats.document/pdfaversion) enumeration values, for example `PdfA1a`, `PdfA1b`, `PdfA2a`, `PdfA2b`, `PdfA2u`, `PdfA3a`, `PdfA3b`, `PdfA3u`, or `None` when the document does not conform to any PDF/A standard.

## Detecting PDF/A type and version information together

The code sample below shows how to retrieve detailed PDF/A conformance information alongside general PDF file format properties.

**AdvancedUsage.DetectingPdfADocumentWithFileFormatInfo**

```csharp
using (Metadata metadata = new Metadata(Constants.InputPdf))
{
	var root = metadata.GetRootPackage<PdfRootPackage>();

	Console.WriteLine("File format : {0}", root.FileType.FileFormat);
	Console.WriteLine("PDF version : {0}", root.FileType.Version);
	Console.WriteLine("MIME type   : {0}", root.FileType.MimeType);
	Console.WriteLine("PDF/A version: {0}", root.FileType.PdfAVersion);
}
```

{{< alert style="info" >}}
PDF/A conformance information is stored in the document's XMP metadata under the `pdfaid` namespace. GroupDocs.Metadata reads this information automatically and exposes it through the `PdfAVersion` property, so you do not need to parse the XMP stream manually.
{{< /alert >}}

## More resources
### GitHub examples
You may easily run the code above and see the feature in action in our GitHub examples:
*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)

### Free online document metadata management App
Along with full featured .NET library we provide simple, but powerful free Apps.
You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
