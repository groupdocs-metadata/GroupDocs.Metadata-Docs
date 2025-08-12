---
id: installation
url: metadata/java/installation
title: Installation
weight: 3
description: ""
keywords:
productName: GroupDocs.Metadata for Java
hideChildren: False
---
## Development environment

### Operating systems

**GroupDocs.Metadata for Java** can be executed on any Operating System with Java JDK installed.

*   Windows Desktops and Servers
*   Linux
*   Mac OS

### Supported runtime

**GroupDocs.Metadata for Java** supports Java run-time version 7 (1.7) and above.

### Development environments

*   NetBeans
*   IntelliJ IDEA
*   Eclipse

## Installation from GroupDocs artifactory using Maven

GroupDocs hosts all Java APIs on [GroupDocs Artifactory](https://releases.groupdocs.com/java/repo/). You can easily use [GroupDocs.Metadata for Java](https://releases.groupdocs.com/java/repo/com/groupdocs/groupdocs-metadata/) API directly in your Maven projects with simple configurations.

### Specify GroupDocs repository configuration

First, you need to specify repository configuration/location in your project as follows:

{{< tabs "example1">}}
{{< tab "Maven" >}}
```xml
<repositories>
	<repository>
		<id>GroupDocs Artifact Repository</id>
        	<name>GroupDocs Artifact Repository</name>
        	<url>https://releases.groupdocs.com/java/repo/</url>
	</repository>
</repositories>
```
{{< /tab >}}
{{< tab "Gradle" >}}
```xml
repositories {
    maven {
        url "https://repository.groupdocs.com/repo/"
    }
}
```
{{< /tab >}}
{{< tab "Kotlin" >}}
```xml
repositories {
    maven(url = "https://repository.groupdocs.com/repo/")
}
```
{{< /tab >}}
{{< tab "Ivy" >}}
```xml
<ivysettings>
    <settings defaultResolver="chain"/>
    <resolvers>
        <chain name="chain">
            <ibiblio name="GroupDocs Repository" m2compatible="true" root="https://releases.groupdocs.com/java/repo/"/>
        </chain>
    </resolvers>
</ivysettings>
```
{{< /tab >}}
{{< tab "Sbt" >}}
```xml
resolvers += Resolver.url("GroupDocs Repository", url("https://releases.groupdocs.com/java/repo/"))
```
{{< /tab >}}
{{< /tabs >}}

### Add GroupDocs.Metadata for Java as a dependency

Then define GroupDocs.Metadata for Java API dependency in your pom.xml as follows:

{{< tabs "example2">}}
{{< tab "Maven" >}}
```xml
<dependencies>
    <dependency>
        <groupId>com.groupdocs</groupId>
        <artifactId>groupdocs-metadata</artifactId>
        <version>24.5</version>
    </dependency>
</dependencies>
```
{{< /tab >}}
{{< tab "Gradle" >}}
```xml
dependencies {
    implementation 'com.groupdocs:groupdocs-metadata:24.5'
}
```
{{< /tab >}}
{{< tab "Kotlin" >}}
```xml
dependencies {
    implementation("com.groupdocs:groupdocs-metadata:24.5")
}
```
{{< /tab >}}
{{< tab "Ivy" >}}
```xml
<dependency org="com.groupdocs" name="groupdocs-metadata" rev="24.5">
   <artifact name="groupdocs-metadata" ext="jar"/>
</dependency>
```
{{< /tab >}}
{{< tab "Sbt" >}}
```xml
libraryDependencies += "com.groupdocs" % "groupdocs-metadata" % "24.5"
```
{{< /tab >}}
{{< /tabs >}}

After performing above-mentioned steps, the GroupDocs.Metadata for Java dependency will finally be added to your Maven project.

_The latest version of Metadata API can be [found here](https://releases.groupdocs.com/java/repo/com/groupdocs/groupdocs-metadata/) _

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
