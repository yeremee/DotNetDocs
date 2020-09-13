---
title: "<caches>"
ms.date: "03/30/2017"
ms.assetid: 4651091b-3a20-40d8-b293-4408c0710143
author: "BrucePerlerMS"
---
# \<caches>
Registers the caches used for session tokens and token replay detection.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<caches>**  
  
## Syntax  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <caches>  
    </caches>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## Attributes and Elements  
 The following sections describe attributes, child elements, and parent elements.  
  
### Attributes  
 None  
  
### Child Elements  
  
|Element|Description|  
|-------------|-----------------|  
|[\<sessionSecurityTokenCache>](sessionsecuritytokencache.md)|Registers a cache for session tokens with a service or a security token handler collection.|  
|[\<tokenReplayCache>](tokenreplaycache.md)|Registers a token replay cache with a service or a security token handler collection.|  
  
### Parent Elements  
  
|Element|Description|  
|-------------|-----------------|  
|[\<identityConfiguration>](identityconfiguration.md)|Specifies service-level identity settings.|  
|[\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md)|Provides configuration for a collection of security token handlers.|  
  
## Remarks  
 A `<caches>` element can be specified at the service level under the `<identityConfiguration>` element or on the security token handler collection level under the `<securityTokenHandlerConfiguration>` element. Settings on a token handler collection override those specified on the service.  
  
 The `<caches>` element is represented by the <xref:System.IdentityModel.Configuration.IdentityModelCachesElement> class. The configured caches are represented by the <xref:System.IdentityModel.Configuration.IdentityModelCaches> class.  
  
## Example  
 The following XML shows the configuration of a custom cache for holding session security tokens (<xref:System.IdentityModel.Tokens.SessionSecurityToken>). The configuration is taken from the `ClaimsAwareWebFarm` sample.  
  
```xml  
<caches>  
  <sessionSecurityTokenCache type="CacheLibrary.SharedSessionSecurityTokenCache, CacheLibrary">  
    <!--cacheServiceAddress points to the centralized session security token cache service running in the web farm.-->  
    <cacheServiceAddress url="http://localhost:4161/SessionSecurityTokenCacheService.svc" />  
  </sessionSecurityTokenCache>  
</caches>  
```
