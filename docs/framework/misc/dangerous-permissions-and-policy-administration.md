---
title: "Dangerous Permissions and Policy Administration"
description: See links to the various dangerous permissions in .NET. These permissions should be given only to trustworthy code, and only when necessary.
ms.date: "03/30/2017"
helpviewer_keywords: 
  - "permissions [.NET Framework], policy administration"
  - "security [.NET Framework], dangerous permissions"
  - "code security, dangerous permissions"
  - "secure coding, dangerous permissions"
  - "permissions [.NET Framework], dangerous"
ms.assetid: 1929e854-23a0-4bb1-94be-e8aa3b609e32
---
# Dangerous Permissions and Policy Administration

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

Several of the protected operations for which the .NET Framework provides permissions can potentially allow the security system to be circumvented. These dangerous permissions should be given only to trustworthy code, and then only as necessary. There is usually no defense against malicious code if it is granted these permissions.  
  
> [!NOTE]
> In the .NET Framework 4, there have been important changes to the .NET Framework security model and terminology. For more information about these changes, see [Security Changes](https://docs.microsoft.com/previous-versions/dotnet/framework/security/security-changes).  
  
 The dangerous permissions are explained in the following table.  
  
|Permission|Potential risk|  
|----------------|--------------------|  
|<xref:System.Security.Permissions.SecurityPermission>||  
|<xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode>|Allows managed code to call into unmanaged code, which is often dangerous.|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.SkipVerification>|Without verification, the code can do anything.|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.ControlEvidence>|Invalidated evidence can fool security policy.|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.ControlPolicy>|The ability to modify security policy can disable security.|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.SerializationFormatter>|The use of serialization can circumvent accessibility mechanisms. For details, see [Security and Serialization](security-and-serialization.md).|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.ControlPrincipal>|The ability to set the current principal can trick role-based security.|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.ControlThread>|Manipulation of threads is dangerous because of the security state associated with threads.|  
|<xref:System.Security.Permissions.ReflectionPermission>||  
|<xref:System.MemberAccessException>|Can use private members to defeat accessibility mechanisms.|  
  
## See also

- [Secure Coding Guidelines](../../standard/security/secure-coding-guidelines.md)
