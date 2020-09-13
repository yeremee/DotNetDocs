---
description: "-nowin32manifest (C# Compiler Options)"
title: "-nowin32manifest (C# Compiler Options)"
ms.date: 07/20/2015
f1_keywords: 
  - "/nowin32manifest"
helpviewer_keywords: 
  - "nowin32manifest compiler option [C#]"
  - "-nowin32manifest compiler option [C#]"
  - "/nowin32manifest compiler option [C#]"
ms.assetid: 6f06365b-b87b-46a2-b187-b3bfeaf4862d
---
# -nowin32manifest (C# Compiler Options)
Use the **-nowin32manifest** option to instruct the compiler not to embed any application manifest into the executable file.  
  
## Syntax  
  
```console  
-nowin32manifest  
```  
  
## Remarks  
 When this option is used, the application will be subject to virtualization on Windows Vista unless you provide an application manifest in a Win32 Resource file or during a later build step.  
  
 In Visual Studio, set this option in the **Application Property** page by selecting the **Create Application Without a Manifest** option in the **Manifest** drop down list. For more information, see [Application Page, Project Designer (C#)](/visualstudio/ide/reference/application-page-project-designer-csharp).  
  
 For more information about manifest creation, see [-win32manifest (C# Compiler Options)](./win32manifest-compiler-option.md).  
  
## See also

- [C# Compiler Options](./index.md)
- [Managing Project and Solution Properties](/visualstudio/ide/managing-project-and-solution-properties)
