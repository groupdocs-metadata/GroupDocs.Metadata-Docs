---
id: working-with-saved-emails
url: metadata/java/working-with-saved-emails
title: Working with saved Emails
weight: 6
description: ""
keywords: 
productName: GroupDocs.Metadata for Java
hideChildren: False
---
GroupDocs.Metadata for Java provides functionality that allows handling the most popular email message formats: EML and MSG. The following are some commonly used scenarios of working with the message content and metadata.

## Reading EML message metadata

The EML message format is used by many email clients including Novell GroupWise, Microsoft Outlook Express, Lotus notes, Windows Mail, Mozilla Thunderbird, and Postbox. The files contain the email contents as plain text in MIME format, containing the email header and body, including attachments in one or more of several formats. The code sample below demonstrates how to extract metadata from an EML message.

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>email.EmlReadNativeMetadataProperties**

```csharp
try (Metadata metadata = new Metadata(Constants.InputEml)) {
	EmlRootPackage root = metadata.getRootPackageGeneric();
	System.out.println(root.getEmailPackage().getSenderEmailAddress());
	System.out.println(root.getEmailPackage().getSubject());

	for (String recipient : root.getEmailPackage().getRecipients()) {
		System.out.println(recipient);
	}

	for (String attachedFileName : root.getEmailPackage().getAttachedFileNames()) {
		System.out.println(attachedFileName);
	}

	for (MetadataProperty header : root.getEmailPackage().getHeaders()) {
		System.out.println(String.format("%s = %s", header.getName(), header.getValue()));
	}

	// ...
}
```

## Reading MSG metadata

MSG files are usually created by the Microsoft Outlook email client. The MSG format is used to represent individual email messages, appointments, contacts, tasks, and so on in the file system. The following code snippet shows how to read most common properties of an email message stored as an MSG file.

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>email.MsgReadNativeMetadataProperties**

```csharp
try (Metadata metadata = new Metadata(Constants.InputMsg)) {
	MsgRootPackage root = metadata.getRootPackageGeneric();
	System.out.println(root.getMsgPackage().getSenderEmailAddress());
	System.out.println(root.getMsgPackage().getSubject());

	for (String recipient : root.getMsgPackage().getRecipients()) {
		System.out.println(recipient);
	}

	for (String attachedFileName : root.getMsgPackage().getAttachedFileNames()) {
		System.out.println(attachedFileName);
	}

	for (MetadataProperty header : root.getMsgPackage().getHeaders()) {
		System.out.println(String.format("%s = %s", header.getName(), header.getValue()));
	}

	System.out.println(root.getMsgPackage().getBody());
	System.out.println(root.getMsgPackage().getDeliveryTime());

	// ...
}
```

## Removing email attachments

Both root package classes representing metadata in email messages ([EmlRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/EmlRootPackage) and [MsgRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/MsgRootPackage)) have the common ancestor which is the [EmailRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/EmailRootPackage) class. Since the MSG and EML formats have really similar functionality, in most cases, you don't need to know the exact type of a loaded message. It is possible to refer to the [EmailRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/EmailRootPackage) class to access all basic features related to email messages. The following code sample shows how to remove all email message attachments using an instance of the [EmailRootPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/EmailRootPackage) class.

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>email.EmailRemoveAttachments**

```csharp
try (Metadata metadata = new Metadata(Constants.InputEml)) {
	EmailRootPackage root = metadata.getRootPackageGeneric();
	root.clearAttachments();
	metadata.save(Constants.OutputEml);
}
```

## Updating email fields

GroupDocs.Metadata for Java also allows the user to update some common email fields. The code sample below demonstrates how to alter the email subject and recipients.

**advanced_usage.managing_metadata_for_specific_formats.email.EmailUpdateEmailFields**

```csharp
try (Metadata metadata = new Metadata(Constants.InputEml)) {
    EmailRootPackage root = metadata.getRootPackageGeneric();
    root.getEmailPackage().setRecipients(new String[] { "sample@aspose.com" });
    root.getEmailPackage().setCarbonCopyRecipients(new String[] { "sample@groupdocs.com" });
    root.getEmailPackage().setSubject("RE: test subject");
    metadata.save(Constants.OutputEml);
}
```

## Updating MSG message fields

An MSG message has more editable fields than the common email package. The [MsgPackage](https://reference.groupdocs.com/metadata/java/com.groupdocs.metadata.core/MsgPackage) class allows you to change the following properties:

*   the body: `setBody`, `setBodyHtml`, `setBodyRtf`;
*   the sender: `setSenderName`, `setSenderSmtpAddress`, `setSentRepresentingEmailAddress`, `setSentRepresentingName`;
*   the times: `setDeliveryTime`, `setClientSubmitTime`;
*   other fields: `setCategories`, `setBilling`, `setMileage`, `setReplyTo`, `setReadReceiptRequested`;
*   the headers, through `getHeaders().set(String, PropertyValue)`, which adds a new header or updates an existing one;
*   the attachments, through `setAttachments`.

Several other MAPI properties can be read but not changed, for example `getInternetMessageId`, `getConversationTopic`, `getDisplayTo`, `getDisplayCc`, `getNormalizedSubject`, and the `isEncrypted`, `isSigned` and `isTemplate` flags.

The code sample below changes the sender, the delivery time, the body, the categories, the headers and the attachments of a message.

**advanced\_usage.managing\_metadata\_for\_specific\_formats.<WBR>email.MsgUpdateEmailFields**

```java
try (Metadata metadata = new Metadata(Constants.InputMsg)) {
    MsgRootPackage root = metadata.getRootPackageGeneric();
    MsgPackage msg = root.getMsgPackage();

    msg.setSenderName("sender");
    msg.setSenderSmtpAddress("sender1@sender.com");
    msg.setDeliveryTime(new Date());
    msg.setBody("text");
    msg.setCategories(new String[]{"test1", "test2"});

    // Add new headers or update the existing ones
    msg.getHeaders().set("X-Custom-Header", new PropertyValue("Custom Value"));
    msg.getHeaders().set("X-SID-Result", new PropertyValue("PASS"));

    // Replace the attachments
    byte[] content = Files.readAllBytes(Paths.get(Constants.AttachmentJpg));
    msg.setAttachments(new MsgAttachmentPackage[]{new MsgAttachmentPackage("tree.jpg", content)});

    metadata.save(Constants.OutputMsg);
}
```

{{< alert style="info" >}}
Starting from version 26.11, the sender address is returned by `getSenderEmailAddress()`, and the sender property is named `SenderEmailAddress` instead of `Sender`. The `getSender()` method is deprecated and returns the same value. If your code looks the property up by name, use the new name.
{{< /alert >}}

## More resources

### GitHub examples

You may easily run the code above and see the feature in action in our GitHub examples:

*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)
    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)
    

### Free online document metadata management App

Along with full featured Java library we provide simple, but powerful free Apps.

You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).
