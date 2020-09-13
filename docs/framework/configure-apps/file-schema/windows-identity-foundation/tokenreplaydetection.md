---
title: "<tokenReplayDetection>"
ms.date: "03/30/2017"
ms.assetid: ac3f588e-5f75-4275-b969-2d492ecc3b47
author: "BrucePerlerMS"
---
# \<tokenReplayDetection>
Enables token replay detection and specifies the expiration time for tokens.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<tokenReplayDetection>**  
  
## Syntax  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <tokenReplayDetection enabled=xs:boolean expirationPeriod=TimeSpan>  
    </tokenReplayDetection>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## Type  
 <xref:System.IdentityModel.Configuration.TokenReplayDetectionElement>  
  
## Attributes and Elements  
 The following sections describe attributes, child elements, and parent elements.  
  
### Attributes  
  
|Attribute|Description|  
|---------------|-----------------|  
|enabled|A value that specifies whether token replay detection is enabled; "true" to enable token replay detection.|  
|expirationPeriod|A <xref:System.TimeSpan> that specifies the maximum amount of time before an item is considered expired and removed from the cache.  For more information about how to specify <xref:System.TimeSpan> values, see [Timespan Values](../windows-workflow-foundation/index.md).|  
  
### Child Elements  
 None  
  
### Parent Elements  
  
|Element|Description|  
|-------------|-----------------|  
|[\<identityConfiguration>](identityconfiguration.md)|Specifies service-level identity settings.|  
|[\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md)|Provides configuration for a collection of security token handlers.|  
  
## Remarks  
 A `<tokenReplayDetection>` element can be specified at the service level under the `<identityConfiguration>` element or on the security token handler collection level under the `<securityTokenHandlerConfiguration>` element. Settings on a token handler collection override those specified on the service.  
  
 The type of the token replay cache is specified by the [\<tokenReplayCache>](tokenreplaycache.md) element.
