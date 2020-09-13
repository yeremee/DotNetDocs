---
title: "<Event> Element (.NET Native)"
ms.date: "03/30/2017"
ms.assetid: e53b029c-9d6d-4c0a-9cdc-5cfca8a5ca47
---
# \<Event> Element (.NET Native)
Applies runtime reflection policy to an event.  
  
## Syntax  
  
```xml  
<Event Name="event_name"
       Browse="policy_type"
       Dynamic="policy_type" />  
```  
  
## Attributes and Elements  
 The following sections describe attributes, child elements, and parent elements.  
  
### Attributes  
  
|Attribute|Attribute type|Description|  
|---------------|--------------------|-----------------|  
|`Name`|General|Required attribute. Specifies the event name.|  
|`Browse`|Reflection|Optional attribute. Controls querying for information about or enumerating the event but does not enable any dynamic access at run time.|  
|`Dynamic`|Reflection|Optional attribute. Controls runtime access to the event to enable dynamic programming. This policy ensures that an event can be handled dynamically at run time.|  
  
## Name attribute  
  
|Value|Description|  
|-----------|-----------------|  
|*method_name*|The event name. The type of the event is defined by the parent [\<Type>](type-element-net-native.md) or [\<TypeInstantiation>](typeinstantiation-element-net-native.md) element.|  
  
## All other attributes  
  
|Value|Description|  
|-----------|-----------------|  
|*policy_setting*|The setting to apply to this policy type for the event. Possible values are `Auto`, `Excluded`, `Included`, and `Required`. For more information, see [Runtime Directive Policy Settings](runtime-directive-policy-settings.md).|  
  
### Child Elements  
 None.  
  
### Parent Elements  
  
|Element|Description|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|Applies reflection policy to a type and all its members.|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|Applies reflection policy to a constructed generic type and all its members.|  
  
## Remarks  
 If an event's policy is not explicitly defined, it inherits the runtime policy of its parent element.  
  
## See also

- [Runtime Directives (rd.xml) Configuration File Reference](runtime-directives-rd-xml-configuration-file-reference.md)
- [Runtime Directive Elements](runtime-directive-elements.md)
- [Runtime Directive Policy Settings](runtime-directive-policy-settings.md)
