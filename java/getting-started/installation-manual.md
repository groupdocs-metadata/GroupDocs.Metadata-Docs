---
id: installation-manual
url: metadata/java/installation-manual
title: Installation
weight: 3
description: ""
keywords:
productName: GroupDocs.Metadata for Java
hideChildren: False
---

## Installation without build tools (manual JAR setup)

If you don’t use Maven/Gradle/Ivy/SBT, you can add **GroupDocs.Metadata for Java** to the classpath manually.

### 1) Download the JAR

1. Go to the latest version of **groupdocs-metadata** in the GroupDocs Java repository.
2. Download `groupdocs-metadata-<version>.jar`.

### 2) Add the JAR to your project

**IntelliJ IDEA**

1. *File* → *Project Structure* → *Modules* → *Dependencies*.
2. Click **+** → **JARs or directories** and select the downloaded JAR.

**Eclipse**

1. Right-click the project → *Properties* → *Java Build Path* → *Libraries*.
2. Click **Add External JARs…** and select the downloaded JAR.

**NetBeans**

1. Right-click *Libraries* in your project.
2. Choose **Add JAR/Folder…** and select the downloaded JAR.

> You can place the JAR anywhere in your project (e.g., `lib/`) and add it to the module/classpath.

### 3) Compile and run from the command line

Place the JAR under `lib/` (or another folder) and use the appropriate classpath separator for your OS.

**Windows (CMD/PowerShell):**

```bash
javac -cp .;lib\groupdocs-metadata-<version>.jar Example.java
java  -cp .;lib\groupdocs-metadata-<version>.jar Example path\to\file.pdf
```

**macOS/Linux (bash/zsh):**

```bash
javac -cp .:lib/groupdocs-metadata-<version>.jar Example.java
java  -cp .:lib/groupdocs-metadata-<version>.jar Example /path/to/file.pdf
```

### 4) Minimal example to verify the setup

```java
import com.groupdocs.metadata.Metadata;
import com.groupdocs.metadata.core.FileFormat;
import com.groupdocs.metadata.core.IDocumentInfo;

public class Example {
    public static void main(String[] args) {
        if (args.length == 0) {
            System.out.println("Usage: java Example <path-to-file>");
            return;
        }
        try (Metadata metadata = new Metadata(args[0])) {
            if (metadata.getFileFormat() != FileFormat.Unknown) {
                IDocumentInfo info = metadata.getDocumentInfo();
                System.out.println("Format: " + info.getFileType().getFileFormat());
                System.out.println("Extension: " + info.getFileType().getExtension());
                System.out.println("Pages: " + info.getPageCount());
                System.out.println("Size (bytes): " + info.getSize());
                System.out.println("Encrypted: " + info.isEncrypted());
            }
        }
    }
}
```

### 5) Licensing (optional)

You can use the library in trial mode with evaluation limitations. To remove limitations, apply a license (file or Metered) in your application according to the **Licensing** guide.
