---
id: detecting-pdf-a-document
url: metadata/java/detecting-pdf-a-document
title: Detecting PDF/A document
weight: 8
description: "This article shows how to detect whether a PDF file is PDF/A compliant and determine its conformance level in Java programmatically with GroupDocs.Metadata for Java"
keywords: Java PDF PDF/A detect conformance archiving ISO compliance Metadata
productName: GroupDocs.Metadata for Java
hideChildren: False
---
PDF/A is an ISO-standardized version of PDF for digital preservation and long-term archiving of electronic documents. Unlike standard PDF, PDF/A prohibits features unsuitable for long-term archiving, such as font linking and encryption, and requires that certain features be included, such as embedded fonts and color profiles.

There are several conformance levels defined across multiple PDF/A versions:

*   **PDF/A-1** (ISO 19005-1) — based on PDF 1.4, with conformance levels **a** (accessible) and **b** (basic)
*   **PDF/A-2** (ISO 19005-2) — based on PDF 1.7, adding conformance level **u** (Unicode)
*   **PDF/A-3** (ISO 19005-3) — same as PDF/A-2 but allows embedding arbitrary file formats as attachments
*   **PDF/A-4** (ISO 19005-4) — based on PDF 2.0, with the **e** (engineering) and **f** (embedded files) variants

Using GroupDocs.Metadata for Java you can detect whether a loaded PDF document conforms to a PDF/A standard and find out its exact version and conformance level.

## Detecting PDF/A conformance

The following steps and Java code sample show **how to detect the PDF/A type of a PDF document in a Java application**:

1.  [Load]({{< ref "metadata/java/developer-guide/advanced-usage/loading-files/_index.md" >}}) a PDF document
2.  Extract the root metadata package
3.  Get the file format package using the [getPdfType](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/PdfRootPackage#getPdfType()) method
4.  Check the [isPdfA](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/PdfTypePackage#isPdfA()) flag and read the [getPdfFormat](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/PdfTypePackage#getPdfFormat()) value

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>document.pdf.<WBR>PdfDetectPdfA**

```java
try (Metadata metadata = new Metadata(Constants.InputPdfA)) {
    PdfRootPackage root = metadata.getRootPackageGeneric();

    if (root.getPdfType().isPdfA()) {
        System.out.println(String.format("PDF/A version: %s", root.getPdfType().getPdfFormat()));
    } else {
        System.out.println("The document is not PDF/A compliant.");
    }
}
```

For a PDF/A-1b document the code prints `PDF/A version: PdfA1B`.

The [getPdfFormat](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/PdfTypePackage#getPdfFormat()) method returns one of the [PdfFormat](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/PdfFormat) enumeration values:

| Group | Values |
|-------|--------|
| PDF/A | `PdfA1A`, `PdfA1B`, `PdfA2A`, `PdfA2B`, `PdfA2U`, `PdfA3A`, `PdfA3B`, `PdfA3U`, `PdfA4`, `PdfA4E`, `PdfA4F` |
| PDF/X | `PdfX1A2001`, `PdfX1A`, `PdfX3`, `PdfX4` |
| Plain PDF | `Pdf10`, `Pdf11`, `Pdf12`, `Pdf13`, `Pdf14`, `Pdf15`, `Pdf16`, `Pdf17`, `Pdf20` |
| Other | `PdfUa1`, `ZugFeRD`, `Unknown` |

[isPdfA](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/PdfTypePackage#isPdfA()) returns `true` for any value from the PDF/A group, and [isPdfX](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/PdfTypePackage#isPdfX()) returns `true` for any value from the PDF/X group.

## Detecting PDF/A type and version information together

The code sample below shows how to read the PDF/A conformance information together with general PDF file format properties.

```java
try (Metadata metadata = new Metadata(Constants.InputPdfA)) {
    PdfRootPackage root = metadata.getRootPackageGeneric();
    PdfTypePackage pdfType = root.getPdfType();

    System.out.println(String.format("File format : %s", pdfType.getFileFormat()));
    System.out.println(String.format("PDF version : %s", pdfType.getVersion()));
    System.out.println(String.format("MIME type   : %s", pdfType.getMimeType()));
    System.out.println(String.format("PDF format  : %s", pdfType.getPdfFormat()));
    System.out.println(String.format("Is PDF/A    : %s", pdfType.isPdfA()));
    System.out.println(String.format("Is PDF/X    : %s", pdfType.isPdfX()));
}
```

## Checking a folder of PDF documents

Before PDF files are moved to an archive, it is common to check which of them are already PDF/A. The following code sample prints the detected format of every PDF file in a folder.

```java
File folder = new File(Constants.InputPath);
for (File file : folder.listFiles((dir, name) -> name.toLowerCase().endsWith(".pdf"))) {
    try (Metadata metadata = new Metadata(file.getAbsolutePath())) {
        PdfRootPackage root = metadata.getRootPackageGeneric();
        PdfFormat format = root.getPdfType().getPdfFormat();

        if (root.getPdfType().isPdfA()) {
            System.out.println(String.format("%s: %s", file.getName(), format));
        } else {
            System.out.println(String.format("%s: not PDF/A (%s)", file.getName(), format));
        }
    }
}
```

{{< alert style="info" >}}
GroupDocs.Metadata reports the conformance level that the document declares about itself. The check does not validate the file against the PDF/A standard, so use a dedicated validator if you need proof of compliance.
{{< /alert >}}

## More resources
### GitHub examples
You may easily run the code above and see the feature in action in our GitHub examples:
*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)

### Free online document metadata management App
Along with full featured Java library we provide simple, but powerful free Apps.
You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
