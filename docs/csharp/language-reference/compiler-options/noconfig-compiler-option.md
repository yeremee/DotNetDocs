---
description: "-noconfig (C# Compiler Options)"
title: "-noconfig (C# Compiler Options)"
ms.date: 07/20/2015
f1_keywords: 
  - "/noconfig"
helpviewer_keywords: 
  - "/noconfig compiler option [C#]"
  - "csc.rsp"
  - "-noconfig compiler option [C#]"
  - "noconfig compiler option [C#]"
ms.assetid: cd26967e-e494-4c8c-b5c9-af13b2f78b2e
---
# -noconfig (C# Compiler Options)
The **-noconfig** option tells the compiler not to compile with the csc.rsp file, which is located in and loaded from the same directory as the csc.exe file.  
  
## Syntax  
  
```console  
-noconfig  
```  
  
## Remarks  
 The csc.rsp file references all the assemblies shipped with .NET Framework. The actual references that the Visual Studio .NET development environment includes depend on the project type.  
  
 You can modify the csc.rsp file and specify additional compiler options that should be included in every compilation from the command line with csc.exe (except the **-noconfig** option).  
  
 The compiler processes the options passed to the **csc** command last. Therefore, any option on the command line overrides the setting of the same option in the csc.rsp file.  
  
 If you do not want the compiler to look for and use the settings in the csc.rsp file, specify **-noconfig**.  
  
 This compiler option is unavailable in Visual Studio and cannot be changed programmatically.  
  
## See also

- [C# Compiler Options](./index.md)
- [Managing Project and Solution Properties](/visualstudio/ide/managing-project-and-solution-properties)
