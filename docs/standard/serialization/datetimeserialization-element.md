---
title: "<dateTimeSerialization> Element"
description: This article describes the <dateTimeSerialization> element, which determines the serialization mode of DateTime objects.
ms.date: "03/30/2017"
helpviewer_keywords: 
  - "dateTimeSerialization element"
  - "XML serialization, configuration"
  - "<dateTimeSerialization> element"
ms.assetid: 90fda55c-7730-41e9-bc4b-6423a4b920af
---
# \<dateTimeSerialization> Element
Determines the serialization mode of <xref:System.DateTime> objects.  
  
 \<configuration>  
\<dateTimeSerialization>  
  
## Syntax  
  
```xml  
<dateTimeSerialization  
    mode = "Roundtrip|Local"  
/>  
```  
  
## Attributes and Elements  
 The following sections describe attributes, child elements, and parent elements.  
  
### Attributes  
  
|Attributes|Description|  
|----------------|-----------------|  
|`mode`|Optional. Specifies the serialization mode. Set to one of the <xref:System.Xml.Serialization.Configuration.DateTimeSerializationSection.DateTimeSerializationMode> values. The default is **RoundTrip**.|  
  
### Child Elements  
 None.  
  
### Parent Elements  
  
|Element|Description|  
|-------------|-----------------|  
|system.xml.serialization|The top-level element for controlling XML serialization.|  
  
## Remarks  
 In versions 1.0, 1.1, 2.0 and later versions of the .NET Framework, when this property is set to **Local**, <xref:System.DateTime> objects are always formatted as the local time. That is, local time zone information is always included with the serialized data. Set this property to **Local** to ensure compatibility with older versions of the .NET Framework.  
  
 In version 2.0 and later versions of the .NET Framework that have this property set to **Roundtrip**, <xref:System.DateTime> objects are examined to determine whether they are in the local, UTC, or an unspecified time zone. The <xref:System.DateTime> objects are then serialized in such a way that this information is preserved. This is the default behavior and is the recommended behavior for all new applications that do not communicate with older versions of the framework.  
  
## See also

- <xref:System.DateTime>
- <xref:System.Xml.Serialization.XmlSchemaImporter>
- <xref:System.Xml.Serialization.Configuration.DateTimeSerializationSection.DateTimeSerializationMode>
- [Configuration File Schema](../../framework/configure-apps/file-schema/index.md)
- [\<schemaImporterExtensions> Element](schemaimporterextensions-element.md)
- [\<add> Element for \<schemaImporterExtensions>](add-element-for-schemaimporterextensions.md)
- [\<system.xml.serialization> Element](system-xml-serialization-element.md)
