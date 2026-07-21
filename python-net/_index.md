---
id: home
url: metadata/python-net
title: GroupDocs.Metadata for Python via .NET
linkTitle: GroupDocs.Metadata for Python
weight: 1
description: "Develop Applications to Create, View, Access, Update, Delete, Search, Compare, Replace & Export Metadata of Popular Documents & Image Formats."
keywords: metadata, create, view, access, update, delete, search, compare, replace, export, extract, PDF, PNG, JPEG
productName: GroupDocs.Metadata for Python via .NET
hideChildren: True
fullWidth: True
---
<img src="/logo/128x128/groupdocs-metadata-python.png" alt="groupdocs-metadata-python-home" align="left" style="width:110px; margin: 0 30px 30px 0"/>

<img src="https://img.shields.io/pypi/v/groupdocs-metadata-net?label=GroupDocs.Metadata%20for%20Python%20PyPI
" alt="PyPI package">
<img src="https://img.shields.io/pypi/dm/groupdocs-metadata-net?label=pypi%20downloads" alt="PyPI downloads">

{{< button style="primary" link="https://releases.groupdocs.com/metadata/python-net/release-notes/" >}} <svg class="gdoc-icon gdoc-product-doc__btn-icon"><use xlink:href="/img/groupdocs-stack.svg#document"></use></svg> Release notes {{< /button >}} 
{{< button style="primary" link="https://pypi.org/project/groupdocs-metadata-net" >}} {{< icon "gdoc_download" >}} Package repository {{< /button >}}

GroupDocs.Metadata for Python via .NET is a metadata management API for documents that lets you create, preview, analyze, update, and remove meta information across all popular file formats. It takes a file as input, gives you access to its property information, and lets you perform metadata operations in a unified way regardless of the file format.

GroupDocs.Metadata supports over [110+ popular file formats](/metadata/python-net/supported-document-formats). Load text documents, spreadsheets, presentations, PDF files, email messages, and images.

## Quick example

Install the package with `pip install groupdocs-metadata-net`, then read every metadata property from a document in a few lines:

```python
from groupdocs.metadata import Metadata

# Open a file and read all of its detected metadata properties
with Metadata("input.docx") as metadata:
    for prop in metadata.find_properties(lambda p: True):
        print(f"{prop.name} = {prop.value}")
```

See the [Quick Start Guide]({{< ref "metadata/python-net/getting-started/quick-start-guide.md" >}}) for editing, removing, and exporting metadata.

------

{{< columns >}}
<p><b>About GroupDocs.Metadata</b></p>
<hr><p>OVERVIEW</p></hr>
<ul>
    <li><a href='{{< ref "/metadata/python-net/product-overview.md" >}}'>Product overview</a></li>
    <li><a href='{{< ref "/metadata/python-net/getting-started/features-overview.md" >}}'>Main features</a></li>
    <li><a href='{{< ref "/metadata/python-net/getting-started/supported-document-formats.md" >}}'>Supported file formats</a></li>
</ul>

<p>GET STARTED</p>
<ul>
    <li><a href='{{< ref "/metadata/python-net/getting-started/system-requirements.md" >}}'>System requirements</a></li>
    <li><a href='{{< ref "/metadata/python-net/getting-started/installation.md" >}}'>Installation</a></li>
    <li><a href='{{< ref "/metadata/python-net/getting-started/evaluation-limitations-and-licensing.md" >}}'>Licensing</a></li>
</ul>   

<--->

<p><b>Developer Guide</b></p>
<hr><p>BASICS USAGE</p></hr>
<ul>
    <li><a href='{{< ref "metadata/python-net/developer-guide/basic-usage/get-document-info.md" >}}'>Extract basic format information</a></li>
    <li><a href='{{< ref "metadata/python-net/developer-guide/basic-usage/find-metadata-properties.md" >}}'>Search for specific metadata properties</a></li>
    <li><a href='{{< ref "metadata/python-net/developer-guide/basic-usage/set-metadata-properties.md" >}}'>Set specific metadata properties</a></li>
    <li><a href='{{< ref "metadata/python-net/developer-guide/basic-usage/remove-metadata-properties.md" >}}'>Remove specific metadata properties</a></li>
    <li><a href='{{< ref "metadata/python-net/developer-guide/basic-usage/clean-metadata.md" >}}'>Remove all detected metadata packages/properties</a></li>
</ul>

<p>API REFERENCE</p>
<ul>
    <li><a href="https://reference.groupdocs.com/metadata/python-net/">GroupDocs.Metadata for Python via .NET API Reference</a></li>
</ul>

<--->

<p><b>Useful Resources</b></p>
<hr><p>DEMOS AND EXAMPLES</p></hr>
<ul>
   <li><a href="https://products.groupdocs.app/metadata/total">Work with metadata online</a></li>
    <li><a href="https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET">Download examples and demos from GitHub</a></li>
	<li><a href='{{< ref "/metadata/python-net/getting-started/how-to-run-examples.md" >}}'>How to run examples</a></li>
</ul>

<p>VERSION HISTORY</p>
<ul>
    <li><a href='https://releases.groupdocs.com/metadata/python-net/release-notes/'>GroupDocs.Metadata for Python via .NET Release Notes</a></li>
</ul>

<p>TECHNICAL SUPPORT</p>
<ul>
    <li><a href="https://forum.groupdocs.com/">Free Support Forum for GroupDocs Products</a></li>
    <li><a href="https://helpdesk.groupdocs.com/">Paid Support Helpdesk for GroupDocs Products</a></li>
</ul>

{{< /columns >}}