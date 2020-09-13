---
title: "<chunkedCookieHandler>"
ms.date: "03/30/2017"
ms.assetid: 7220de45-1d14-4aec-a29e-4a2ea8ac861f
author: "BrucePerlerMS"
---
# \<chunkedCookieHandler>
Configures the <xref:System.IdentityModel.Services.ChunkedCookieHandler>. This element may only be present if the `mode` attribute of the `<cookieHandler>` element is "Default" or "Chunked".  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel.services>**](system-identitymodel-services.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<federationConfiguration>**](federationconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<cookieHandler>**](cookiehandler.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<chunkedCookieHandler>**  
  
## Syntax  
  
```xml  
<system.identityModel.services>  
  <federationConfiguration>  
    <cookieHandler mode="Chunked||Default" >  
      <chunkedCookieHandler size=xs:int >  
      </chunkedCookieHandler>  
    </cookieHandler>  
  </federationConfiguration>  
</system.identityModel.services>  
```  
  
## Attributes and Elements  
 The following sections describe attributes, child elements, and parent elements.  
  
### Attributes  
  
|Attribute|Description|  
|---------------|-----------------|  
|chunkSize|The maximum size, in characters, of the HTTP cookie data for any one HTTP cookie. You must be careful when adjusting the chunk size. Web browsers have different limits on the size of cookies and number allowed per domain. For example, the original Netscape specification stipulated these limits: 300 cookies total, 4096 bytes per cookie header (including metadata, not just the cookie value), and 20 cookies per domain. The default is 2000. Required.|  
  
### Child Elements  
 None  
  
### Parent Elements  
  
|Element|Description|  
|-------------|-----------------|  
|[\<cookieHandler>](cookiehandler.md)|Configures the <xref:System.IdentityModel.Services.CookieHandler> that the <xref:System.IdentityModel.Services.SessionAuthenticationModule> (SAM) uses to read and write cookies.|  
  
## Remarks  
 When you specify a <xref:System.IdentityModel.Services.ChunkedCookieHandler> by setting the `mode` attribute of the `<cookieHandler>` element to "Default" or "Chunked", you can specify the chunk size that the cookie handler uses to read and write cookies by including a `<chunkedCookieHandler>` child element and setting its `chunkSize` attribute. If the `<chunkedCookieHandler>` element is not present, the default chunk size of 2000 bytes is used. This element cannot be specified when the `mode` attribute is set to "Custom".  
  
 The `<chunkedCookieHandler>` element is represented by the <xref:System.IdentityModel.Services.ChunkedCookieHandlerElement> class.  
  
## Example  
 The following example configures a chunked cookie handler that writes cookies in chunks of 3000 bytes.  
  
```xml  
<cookieHandler mode="Chunked">  
    <chunkedCookieHandler chunkSize=3000/>  
</cookieHandler>  
```  
  
## See also

- <xref:System.IdentityModel.Services.ChunkedCookieHandler>
