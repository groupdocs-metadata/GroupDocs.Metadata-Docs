---
id: copying-metadata
url: metadata/java/copying-metadata
title: Copying metadata
weight: 12
description: "This article shows how to copy metadata properties from one document to another with GroupDocs.Metadata for Java."
keywords: Java copy metadata properties between documents copyTo GroupDocs.Metadata
productName: GroupDocs.Metadata for Java
hideChildren: False
---
GroupDocs.Metadata for Java can copy known metadata properties from one document to another. This is useful when a document is regenerated or converted and the new file has to keep the author, dates and other properties of the original.

The [copyTo](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata/Metadata#copyTo(com.groupdocs.metadata.core.MetadataPackage)) method works as follows:

*   The operation is recursive, so it affects all nested packages as well.
*   If a property exists in the destination package, its value is updated.
*   If a known property is missing in the destination package, it is added.
*   If a property exists only in the destination package, it is kept. Call [sanitize]({{< ref "metadata/java/developer-guide/basic-usage/clean-metadata.md" >}}) on the destination document first if you need an exact copy.
*   The source and the destination packages must be of the same type, for example both documents must be PDF files. Otherwise an exception is thrown.

## Copying all known properties

1.  [Load]({{< ref "metadata/java/developer-guide/advanced-usage/loading-files/_index.md" >}}) the source and the destination documents
2.  Get the root package of the destination document
3.  Call the [copyTo](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata/Metadata#copyTo(com.groupdocs.metadata.core.MetadataPackage)) method of the source document
4.  Save the destination document

**advanced\_usage.CopyToMetadata**

```java
try (Metadata sourceMetadata = new Metadata(Constants.InputPdf);
     Metadata destinationMetadata = new Metadata(Constants.CopyPdf)) {
    PdfRootPackage destinationRoot = destinationMetadata.getRootPackageGeneric();

    // Copy all known properties from the source document to the destination package
    sourceMetadata.copyTo(destinationRoot);

    System.out.println(destinationRoot.getDocumentProperties().getAuthor());
    System.out.println(destinationRoot.getDocumentProperties().getCreatedDate());
    System.out.println(destinationRoot.getDocumentProperties().getProducer());

    destinationMetadata.save(Constants.OutputPdf);
}
```

## Copying properties with specific tags

To copy only some of the properties, pass a list of [tags]({{< ref "metadata/java/developer-guide/basic-usage/find-metadata-properties.md" >}}) to the [copyTo](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata/Metadata#copyTo(com.groupdocs.metadata.core.MetadataPackage,java.util.List)) method. Only the properties marked with at least one of the tags are copied. The method returns the number of affected properties.

```java
try (Metadata sourceMetadata = new Metadata(Constants.InputPdf);
     Metadata destinationMetadata = new Metadata(Constants.CopyPdf)) {
    PdfRootPackage destinationRoot = destinationMetadata.getRootPackageGeneric();

    // Copy only the properties marked with the specified tags
    List<PropertyTag> tags = new ArrayList<PropertyTag>();
    tags.add(Tags.getPerson().getCreator());
    tags.add(Tags.getTime().getCreated());

    int affected = sourceMetadata.copyTo(destinationRoot, tags);
    System.out.println(String.format("Properties copied: %d", affected));

    destinationMetadata.save(Constants.OutputPdf);
}
```

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)

*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)

### Free online document metadata management App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
