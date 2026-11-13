---
id: working-with-metadata-in-mobi-e-books
url: metadata/java/working-with-metadata-in-mobi-e-books
title: Working with metadata in MOBI e-books
weight: 9
description: "This article shows how to read the PDB and MOBI headers of MOBI e-books and change the full name of the book with GroupDocs.Metadata for Java."
keywords: Java MOBI e-book metadata PDB header MOBI header full name Kindle GroupDocs.Metadata
productName: GroupDocs.Metadata for Java
hideChildren: False
---
## What is MOBI?

MOBI (Mobipocket) is an e-book format used by early Kindle readers. A MOBI file is a Palm Database (PDB) file: it starts with a PDB header, followed by a MOBI header that describes the book.

GroupDocs.Metadata for Java exposes both headers through the [MobiPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/MobiPackage) class:

*   [PDBHeader](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/PDBHeader): the database name, type and creator, the creation, modification and last backup dates, the number of records, and other database fields.
*   [MobiHeader](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/MobiHeader): the full name of the book, the MOBI type and the text encoding.

## Reading MOBI format-specific properties

1.  [Load]({{< ref "metadata/java/developer-guide/advanced-usage/loading-files/_index.md" >}}) a MOBI e-book
2.  Get the root metadata package
3.  Extract the native metadata package using the [MobiRootPackage.getMobiPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/MobiRootPackage#getMobiPackage()) method
4.  Read the properties of the PDB and MOBI headers

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>ebook.MobiReadProperties**

```java
try (Metadata metadata = new Metadata(Constants.InputMobi)) {
    MobiRootPackage root = metadata.getRootPackageGeneric();
    PDBHeader pdbHeader = root.getMobiPackage().getPDBHeader();
    MobiHeader mobiHeader = root.getMobiPackage().getMobiHeader();

    System.out.println(pdbHeader.getName());
    System.out.println(pdbHeader.getType());
    System.out.println(pdbHeader.getCreator());
    System.out.println(pdbHeader.getCreationDate());
    System.out.println(pdbHeader.getModificationDate());

    System.out.println(mobiHeader.getFullName());
    System.out.println(mobiHeader.getMobiType());
    System.out.println(mobiHeader.getTextEncoding());
}
```

The database name in the PDB header is limited to 31 characters, so it often holds a shortened title such as `The_Geography_of_Bliss__One_Gru`. The full title of the book is stored in the MOBI header.

## Updating the full name of a book

The full name of the book can be changed with the [MobiHeader.setFullName](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/MobiHeader#setFullName(java.lang.String)) method.

```java
try (Metadata metadata = new Metadata(Constants.InputMobi)) {
    MobiRootPackage root = metadata.getRootPackageGeneric();

    root.getMobiPackage().getMobiHeader().setFullName("The Updated Title");

    metadata.save(Constants.OutputMobi);
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
