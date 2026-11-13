---
id: working-with-metadata-in-fb2-e-books
url: metadata/java/working-with-metadata-in-fb2-e-books
title: Working with metadata in FB2 e-books
weight: 10
description: "This article shows how to read the title info, document info and publish info of FictionBook 2 (FB2) e-books with GroupDocs.Metadata for Java."
keywords: Java FB2 FictionBook e-book metadata title info authors genres GroupDocs.Metadata
productName: GroupDocs.Metadata for Java
hideChildren: False
---
## What is FB2?

FictionBook 2 (FB2) is an XML-based e-book format. Its metadata is stored in the `<description>` element at the beginning of the file and is divided into several sections.

GroupDocs.Metadata for Java reads these sections through the [Fb2Package](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/Fb2Package) class:

| Section | Getter | Contents |
|---------|--------|----------|
| Title info | `getTitleInfo()` | Book title, authors, translators, genres, keywords, date, language, source language, sequence |
| Source title info | `getSrcTitleInfo()` | The same fields for the original book, if this book is a translation |
| Document info | `getDocumentInfo()` | Authors of the FB2 file, program used, date, source URL, ID and version |
| Publish info | `getPublishInfo()` | Book name, publisher, city, year, ISBN and sequence of the paper edition |
| Custom info | `getCustomInfo()` | Any additional information |

The FB2 format is supported for reading only.

## Reading FB2 metadata

1.  [Load]({{< ref "metadata/java/developer-guide/advanced-usage/loading-files/_index.md" >}}) an FB2 e-book
2.  Get the root metadata package
3.  Extract the native metadata package using the [Fb2RootPackage.getFb2Package](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/Fb2RootPackage#getFb2Package()) method
4.  Read the sections you need. The optional sections can be `null`.

```java
try (Metadata metadata = new Metadata(Constants.InputFb2)) {
    Fb2RootPackage root = metadata.getRootPackageGeneric();
    Fb2Package fb2 = root.getFb2Package();

    Fb2TitleInfo titleInfo = fb2.getTitleInfo();
    System.out.println(titleInfo.getBookTitle());
    System.out.println(titleInfo.getLang());
    System.out.println(titleInfo.getKeywords());
    for (Fb2Author author : titleInfo.getAuthors()) {
        System.out.println(String.format("%s %s", author.getFirstName(), author.getLastName()));
    }
    for (Fb2Genre genre : titleInfo.getGenres()) {
        System.out.println(genre);
    }

    if (fb2.getDocumentInfo() != null) {
        System.out.println(fb2.getDocumentInfo().getProgramUsed());
        System.out.println(fb2.getDocumentInfo().getDate());
        System.out.println(fb2.getDocumentInfo().getId());
    }

    if (fb2.getPublishInfo() != null) {
        System.out.println(fb2.getPublishInfo().getPublisher());
        System.out.println(fb2.getPublishInfo().getYear());
        System.out.println(fb2.getPublishInfo().getIsbn());
    }
}
```

Genres are returned as [Fb2Genre](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/Fb2Genre) enumeration values that match the genre codes of the FB2 specification, for example `sf_space` or `prose_classic`.

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)

*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)

### Free online document metadata management App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
