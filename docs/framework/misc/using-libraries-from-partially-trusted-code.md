---
title: "Using Libraries from Partially Trusted Code"
description: Know how to use libraries from partially trusted code. Use the AllowPartiallyTrustedCallersAttribute attribute to call shared managed libraries.
ms.date: "03/30/2017"
helpviewer_keywords: 
  - "security [.NET Framework], partially trusted code"
  - "partially trusted code"
  - "partial trust"
  - "AllowPartiallyTrustedCallersAttribute attribute"
  - "code access security, partially trusted code"
  - "APTCA"
ms.assetid: dd66cd4c-b087-415f-9c3e-94e3a1835f74
---
# Using Libraries from Partially Trusted Code

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]  
  
> [!NOTE]
> This topic addresses the behavior of strong-named assemblies and applies only to [Level 1](security-transparent-code-level-1.md) assemblies. [Security-Transparent Code, Level 2](security-transparent-code-level-2.md) assemblies in the .NET Framework 4 or later are not affected by strong names. For more information about changes to the security system, see [Security Changes](https://docs.microsoft.com/previous-versions/dotnet/framework/security/security-changes).  
  
 Applications that receive less than full trust from their host or sandbox are not allowed to call shared managed libraries unless the library writer specifically allows them to through the use of the <xref:System.Security.AllowPartiallyTrustedCallersAttribute> attribute. Therefore, application writers must be aware that some libraries will not be available to them from a partially trusted context. By default, all code that executes in a partial-trust [sandbox](how-to-run-partially-trusted-code-in-a-sandbox.md) and is not in the list of full-trust assemblies is partially trusted. If you do not expect your code to be executed from a partially trusted context or to be called by partially trusted code, you do not have to be concerned about the information in this section. However, if you write code that must interact with partially trusted code or operate from a partially trusted context, you should consider the following factors:  
  
- Libraries must be signed with a strong name in order to be shared by multiple applications. Strong names allow your code to be placed in the global assembly cache or added to the full-trust list of a sandboxing <xref:System.AppDomain>, and allow consumers to verify that a particular piece of mobile code actually originates from you.  
  
- By default, strong-named [Level 1](security-transparent-code-level-1.md) shared libraries perform an implicit [LinkDemand](link-demands.md) for full trust automatically, without the library writer having to do anything.  
  
- If a caller does not have full trust but still tries to call such a library, the runtime throws a <xref:System.Security.SecurityException> and the caller is not allowed to link to the library.  
  
- In order to disable the automatic **LinkDemand** and prevent the exception from being thrown, you can place the **AllowPartiallyTrustedCallersAttribute** attribute on the assembly scope of a shared library. This attribute allows your libraries to be called from partially trusted managed code.  
  
- Partially trusted code that is granted access to a library with this attribute is still subject to further restrictions defined by the <xref:System.AppDomain>.  
  
- There is no programmatic way for partially trusted code to call a library that does not have the **AllowPartiallyTrustedCallersAttribute** attribute.  
  
 Libraries that are private to a specific application do not require a strong name or the **AllowPartiallyTrustedCallersAttribute** attribute and cannot be referenced by potentially malicious code outside the application. Such code is protected against intentional or unintentional misuse by partially trusted mobile code without the developer having to do anything extra.  
  
 You should consider explicitly enabling use by partially trusted code for the following types of code:  
  
- Code that has been diligently tested for security vulnerabilities and is in compliance with the guidelines described in [Secure Coding Guidelines](../../standard/security/secure-coding-guidelines.md).  
  
- Strong-named code libraries that are specifically written for partially trusted scenarios.  
  
- Any components (whether partially or fully trusted) signed with a strong name that will be called by code that is downloaded from the Internet.  
  
> [!NOTE]
> Some classes in the .NET Framework class library do not have the **AllowPartiallyTrustedCallersAttribute** attribute and cannot be called by partially trusted code.  
  
## See also

- [Code Access Security](code-access-security.md)
