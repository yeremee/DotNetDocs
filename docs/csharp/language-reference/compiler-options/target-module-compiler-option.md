---
description: "-target:module (C# Compiler Options)"
title: "-target:module (C# Compiler Options)"
ms.date: 07/20/2015
f1_keywords: 
  - "/target:module"
helpviewer_keywords: 
  - "-target compiler options [C#], /target:module"
  - "target compiler options [C#], /target:module"
  - "/target compiler options [C#], /target:module"
ms.assetid: 9af1e4fa-c749-44e7-ae58-90a3d05d4e72
---
# -target:module (C# Compiler Options)
This option causes the compiler to not generate an assembly manifest.  
  
## Syntax  
  
```console  
-target:module  
```  
  
## Remarks  
 By default, the output file created by compiling with this option will have an extension of .netmodule.  
  
 A file that does not have an assembly manifest cannot be loaded by the .NET runtime. However, such a file can be incorporated into the assembly manifest of an assembly by means of [-addmodule](./addmodule-compiler-option.md).  
  
 If more than one module is created in a single compilation, [internal](../keywords/internal.md) types in one module will be available to other modules in the compilation. When code in one module references `internal` types in another module, then both modules must be incorporated into an assembly manifest, by means of **-addmodule**.  
  
 Creating a module is not supported in the Visual Studio development environment.  
  
 For information on how to set this compiler option programmatically, see <xref:VSLangProj80.ProjectProperties3.OutputType%2A>.  
  
## Example  
 Compile `in.cs`, creating `in.netmodule`:  
  
```console  
csc -target:module in.cs  
```  
  
## See also

- [-target (C# Compiler Options)](./target-compiler-option.md)
- [C# Compiler Options](./index.md)
