---
id: installation
url: metadata/java/installation
title: Installation (build tools)
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

{{< alert style="warning" >}}

**Do not install from Maven Central.** Maven Central may list an outdated `com.groupdocs:groupdocs-metadata` release (for example **18.5**). Install the on-prem Java SDK from `https://releases.groupdocs.com/java/repo/` with coordinates `com.groupdocs:groupdocs-metadata`. Use version **26.1**.

This is the **SDK** artifact. For GroupDocs.Metadata Cloud on Java, use `com.groupdocs:groupdocs-metadata-cloud` from `https://repository.groupdocs.cloud/maven/` instead (`groupdocs-metadata-cloud`).

{{< /alert >}}


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
        url "https://releases.groupdocs.com/java/repo/"
    }
}
```
{{< /tab >}}
{{< tab "Kotlin" >}}
```xml
repositories {
    maven(url = "https://releases.groupdocs.com/java/repo/")
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
        <version>26.1</version>
    </dependency>
</dependencies>
```
{{< /tab >}}
{{< tab "Gradle" >}}
```xml
dependencies {
    implementation 'com.groupdocs:groupdocs-metadata:26.1'
}
```
{{< /tab >}}
{{< tab "Kotlin" >}}
```xml
dependencies {
    implementation("com.groupdocs:groupdocs-metadata:26.1")
}
```
{{< /tab >}}
{{< tab "Ivy" >}}
```xml
<dependency org="com.groupdocs" name="groupdocs-metadata" rev="26.1">
   <artifact name="groupdocs-metadata" ext="jar"/>
</dependency>
```
{{< /tab >}}
{{< tab "Sbt" >}}
```xml
libraryDependencies += "com.groupdocs" % "groupdocs-metadata" % "26.1"
```
{{< /tab >}}
{{< /tabs >}}

After performing above-mentioned steps, the GroupDocs.Metadata for Java dependency will finally be added to your Maven project.

_The latest version of Metadata API can be [found here](https://releases.groupdocs.com/java/repo/com/groupdocs/groupdocs-metadata/) _
