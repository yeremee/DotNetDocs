---
description: "-win32icon (C# Compiler Options)"
title: "-win32icon (C# Compiler Options)"
ms.date: 07/20/2015
f1_keywords: 
  - "/win32icon"
helpviewer_keywords: 
  - "win32icon compiler option [C#]"
  - "/win32icon compiler option [C#]"
  - "-win32icon compiler option [C#]"
ms.assetid: 756d9b6d-ab07-41b7-ba58-5bd88f711138
---
# -win32icon (C# Compiler Options)
The **-win32icon** option inserts an .ico file in the output file, which gives the output file the desired appearance in the File Explorer.  
  
## Syntax  
  
```console  
-win32icon:filename  
```  
  
## Arguments  
 `filename`  
 The .ico file that you want to add to your output file.  
  
## Remarks  
 An .ico file can be created with the [Resource Compiler](/windows/desktop/menurc/resource-compiler). The Resource Compiler is invoked when you compile a Visual C++ program; an .ico file is created from the .rc file.  
  
 See [-linkresource](./linkresource-compiler-option.md) (to reference) or [-resource](./resource-compiler-option.md) (to attach) a .NET Framework resource file. See [-win32res](./win32res-compiler-option.md) to import a .res file.  
  
### To set this compiler option in the Visual Studio development environment  
  
1. Open the project's **Properties** pages.  
  
2. Click the **Application** property page.  
  
3. Modify the **Application icon** property.  
  
 For information on how to set this compiler option programmatically, see <xref:VSLangProj80.ProjectProperties3.ApplicationIcon%2A>.  
  
## Example  
 Compile `in.cs` and attach an .ico file `rf.ico` to produce `in.exe`:  
  
```console  
csc -win32icon:rf.ico in.cs  
```  
  
## See also

- [C# Compiler Options](./index.md)
- [Managing Project and Solution Properties](/visualstudio/ide/managing-project-and-solution-properties)
