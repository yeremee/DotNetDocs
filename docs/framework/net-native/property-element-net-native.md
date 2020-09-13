---
title: "<Property> Element (.NET Native)"
ms.date: "03/30/2017"
ms.assetid: ad4ba56d-3bcb-4c10-ba90-1cc66e2175a1
---
# \<Property> Element (.NET Native)
Applies runtime reflection policy to a property.  
  
## Syntax  
  
```xml  
<Property Name="property_name"  
          Browse="policy_type"  
          Dynamic="policy_type"  
          Serialize="policy_type" />  
```  
  
## Attributes and Elements  
 The following sections describe attributes, child elements, and parent elements.  
  
### Attributes  
  
|Attribute|Attribute type|Description|  
|---------------|--------------------|-----------------|  
|`Name`|General|Required attribute. Specifies the property name.|  
|`Browse`|Reflection|Optional attribute. Controls querying for information about or enumerating the property but does not enable any dynamic access at run time.|  
|`Dynamic`|Reflection|Optional attribute. Controls runtime access to the property to enable dynamic programming. This policy ensures that a property can be set or retrieved dynamically at run time.|  
|`Serialize`|Serialization|Optional attribute. Controls runtime access to a property to enable type instances to be serialized by libraries such as the Newtonsoft JSON serializer or to be used for data binding.|  
  
## Name attribute  
  
|Value|Description|  
|-----------|-----------------|  
|*method_name*|The property name. The type of the property is defined by the parent [\<Type>](type-element-net-native.md) or [\<TypeInstantiation>](typeinstantiation-element-net-native.md) element.|  
  
## All other attributes  
  
|Value|Description|  
|-----------|-----------------|  
|*policy_setting*|The setting to apply to this policy type for the property. Possible values are `Auto`, `Excluded`, `Included`, and `Required`. For more information, see [Runtime Directive Policy Settings](runtime-directive-policy-settings.md).|  
  
### Child Elements  
 None.  
  
### Parent Elements  
  
|Element|Description|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|Applies reflection policy to a type and all its members.|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|Applies reflection policy to a constructed generic type and all its members.|  
  
## Remarks  
 If a property's policy is not explicitly defined, it inherits the runtime policy of its parent element.  
  
## Example  
 The following example uses reflection to instantiate a `Book` object and display its property values. The original default.rd.xml file for the project appears as follows:  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
   <Application>  
      <Namespace Name="LibraryApplications"  Browse="Required Public" >  
         <Type Name="Book"   Activate="All" />  
      </Namespace>  
   </Application>  
</Directives>  
```  
  
 The file applies the `All` value to the `Activate` policy for the `Book` class, which allows access to class constructors through reflection. The `Browse` policy for the `Book` class is inherited from its parent namespace. This is set to `Required Public`, which makes metadata available at runtime.  
  
 The following is the source code for the example. The `outputBlock` variable represents a <xref:Windows.UI.Xaml.Controls.TextBlock> control.  
  
 [!code-csharp[ProjectN_Reflection#6](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/property1.cs#6)]  
  
 However, compiling and executing this example throws a [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) exception. Although we've made metadata for the `Book` type available, we've failed to make the implementations of the property getters available dynamically. We can correct this error by either in one of two ways:  
  
- by defining the `Dynamic` policy for the `Book` type in its [\<Type>](type-element-net-native.md) element.  
  
- By adding a nested [\<Property>](property-element-net-native.md) element for each property whose getter we'd like to invoke, as the following default.rd.xml file does.  
  
    ```xml  
    <Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
       <Application>  
          <Namespace Name="LibraryApplications"  Browse="Required Public" >  
             <Type Name="Book"   Activate="All" >  
                <Property Name="Title" Dynamic="Required" />  
                <Property Name="Author" Dynamic="Required" />  
                  <Property Name="ISBN" Dynamic="Required" />  
             </Type>  
          </Namespace>  
       </Application>  
    </Directives>  
    ```  
  
## See also

- [Runtime Directives (rd.xml) Configuration File Reference](runtime-directives-rd-xml-configuration-file-reference.md)
- [Runtime Directive Elements](runtime-directive-elements.md)
- [Runtime Directive Policy Settings](runtime-directive-policy-settings.md)
