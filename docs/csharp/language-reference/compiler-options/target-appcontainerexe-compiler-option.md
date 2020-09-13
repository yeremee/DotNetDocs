---
description: "-target:appcontainerexe (C# Compiler Options)"
title: "-target:appcontainerexe (C# Compiler Options)"
ms.date: 07/20/2015
ms.assetid: e7e62229-23ea-4e53-bef5-380d951bf95f
---
# -target:appcontainerexe (C# Compiler Options)
If you use the **-target:appcontainerexe** compiler option, the compiler creates a Windows executable (.exe) file that must be run in an app container. This option is equivalent to [-target:winexe](./target-winexe-compiler-option.md) but is designed for Windows 8.x Store apps.  
  
## Syntax  
  
```console  
-target:appcontainerexe  
```  
  
## Remarks  
 To require the app to run in an app container, this option sets a bit in the [Portable Executable](/windows/desktop/Debug/pe-format) (PE) file. When that bit is set, an error occurs if the CreateProcess method tries to launch the executable file outside an app container.  
  
 Unless you use the [-out](./out-compiler-option.md) option, the output file name takes the name of the input file that contains the [Main](../../programming-guide/main-and-command-args/index.md) method.  
  
 When you specify this option at a command prompt, all files until the next **-out** or **-target** option are used to create the executable file.  
  
### To set this compiler option in the IDE  
  
1. In **Solution Explorer**, open the shortcut menu for your project, and then choose **Properties**.  
  
2. On the **Application** tab, in the **Output type** list, choose **Windows Store App**.  
  
     This option is available only for Windows 8.x Store app templates.  
  
 For information about how to set this compiler option programmatically, see <xref:VSLangProj80.ProjectProperties3.OutputType%2A>.  
  
## Example  
 The following command compiles `filename.cs` into a Windows executable file that can be run only in an app container.  
  
```console  
csc -target:appcontainerexe filename.cs  
```  
  
## See also

- [-target (C# Compiler Options)](./target-compiler-option.md)
- [-target:winexe (C# Compiler Options)](./target-winexe-compiler-option.md)
- [C# Compiler Options](./index.md)
