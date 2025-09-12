---
id: extracting-property-values
url: metadata/python-net/extracting-property-values
title: Extracting property values
weight: 2
description: "Learn common ways to read metadata property values and handle types in GroupDocs.Metadata for Python via .NET."
keywords: extract property values, read metadata values
productName: GroupDocs.Metadata for Python via .NET
hideChildren: False
---
The most common way to get the metadata property value is to check its type and convert or read it accordingly to a platform-specific type.

**advanced_usage.extracting_property_values.extract_using_type**


```python
def run():
    with gm.Metadata(constants.input_docx) as metadata:
        # Fetch all metadata properties from the file
        properties = metadata.find_properties(lambda p: True)
        for prop in properties:
            # Process string and datetime properties only (as an example)
            if prop.value.type == gm.common.MetadataPropertyType.STRING:
                print(prop.value.to_class(str))
            elif prop.value.type == gm.common.MetadataPropertyType.DATE_TIME:
                # Provide a default if conversion fails
                print(prop.value.to_struct(datetime.min))
```


If it's necessary to process many supported types generically, you can inspect `prop.value.raw_value` and `prop.value.type` and branch accordingly. This lets you handle arrays, numbers, booleans, nested packages, etc., in a single pass.

## More resources
### GitHub examples
You may easily run the code above and see the feature in action in our GitHub examples:
*   [GroupDocs.Metadata for .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-.NET)    
*   [GroupDocs.Metadata for Java examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Java)    
*   [GroupDocs.Metadata for Python via .NET examples](https://github.com/groupdocs-metadata/GroupDocs.Metadata-for-Python-via-.NET/)

### Free online document metadata management App
Along with full featured Python via .NET library we provide simple, but powerful free Apps.
You are welcome to view and edit metadata of PDF, DOC, DOCX, PPT, PPTX, XLS, XLSX, emails, images and more with our free online [Free Online Document Metadata Viewing and Editing App](https://products.groupdocs.app/metadata).


