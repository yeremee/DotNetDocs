---
title: "<Library> Element (.NET Native)"
ms.date: "03/30/2017"
ms.assetid: f642276b-33fb-4a81-b882-8808c31ba69e
---
# \<Library> Element (.NET Native)
Defines the assembly that contains types and type members whose metadata is available for reflection at run time.  
  
 \<Directives> Element  
\<Library> Element  
  
## Syntax  
  
```xml  
<Library Name="assembly_name" />  
```  
  
## Attributes and Elements  
 The following sections describe attributes, child elements, and parent elements.  
  
### Attributes  
  
|Attribute|Description|  
|---------------|-----------------|  
|`Name`|Required attribute. Specifies the name of an assembly. Child elements of this `<Library>` element define the runtime reflection policy for types and type members found in this assembly.|  
  
## Name attribute  
  
|Value|Description|  
|-----------|-----------------|  
|*assembly_name*|The simple name of the assembly, without its file extension. This attribute corresponds to the <xref:System.Reflection.AssemblyName.Name%2A?displayProperty=nameWithType> property. For example, the name of an assembly named Extensions.dll is "Extensions". See the Remarks section for a special form of *assembly_name* that supports conditional inclusion of metadata from the assembly.|  
  
### Child Elements  
  
|Element|Description|  
|-------------|-----------------|  
|[\<Assembly>](assembly-element-net-native.md)|Applies policy to all the types in a particular assembly.|  
|[\<Namespace>](namespace-element-net-native.md)|Applies policy to all the types in a particular namespace.|  
|[\<Type>](type-element-net-native.md)|Applies policy to a particular type, such as a class or structure.|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|Applies policy to a constructed generic type. For example, a [\<TypeInstantiation>](typeinstantiation-element-net-native.md) element could be used to define policy for a `List<String>` type.|  
  
### Parent Elements  
  
|Element|Description|  
|-------------|-----------------|  
|[\<Directives>](directives-element-net-native.md)|The root element of a runtime directives file.|  
  
## Remarks  
 The [\<Directives>](directives-element-net-native.md) element can contain zero, one, or more `<Library>` elements.  
  
 The `<Library>` element serves as a container to define the program elements whose metadata is needed at run time; this element doesn't express policy. At compile time, compiler tools search only the library designated by the `<Library>` element for program elements identified by its child elements. In contrast, compiler tools search all libraries, including.NET Framework core libraries, for program elements identified by child elements of the [\<Application>](application-element-net-native.md) element.  
  
 `<Library>` directives may be conditionally utilized. If the name of the `<Library>` element starts and ends with an asterisk (\*), the `<Library>` directive has an effect only if the assembly specified between the asterisks is referenced by the app. For example, the following runtime directive applies only if the Utilities.dll assembly is referenced by the app.  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
  <Library Name="*Utilities*">  
   ...  
  </Library>  
</Directives>  
```  
  
## See also

- [\<Application> Element](application-element-net-native.md)
- [\<Directives> Element](directives-element-net-native.md)
- [Runtime Directives (rd.xml) Configuration File Reference](runtime-directives-rd-xml-configuration-file-reference.md)
- [Runtime Directive Elements](runtime-directive-elements.md)
