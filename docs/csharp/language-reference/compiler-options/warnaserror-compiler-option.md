---
description: "-warnaserror (C# Compiler Options)"
title: "-warnaserror (C# Compiler Options)"
ms.date: 07/20/2015
f1_keywords: 
  - "/warnaserror"
helpviewer_keywords: 
  - "/warnaserror compiler option [C#]"
  - "-warnaserror compiler option [C#]"
  - "warnaserror compiler option [C#]"
ms.assetid: 04680ec3-08d6-4e2e-a274-38310e10e33c
---
# -warnaserror (C# Compiler Options)
The **-warnaserror+** option treats all warnings as errors  
  
## Syntax  
  
```console  
-warnaserror[+ | -][:warning-list]  
```  
  
## Remarks  
 Any messages that would ordinarily be reported as warnings are instead reported as errors, and the build process is halted (no output files are built).  
  
 By default, **-warnaserror-** is in effect, which causes warnings to not prevent the generation of an output file. **-warnaserror**, which is the same as **-warnaserror+**, causes warnings to be treated as errors.  
  
 Optionally, if you want only a few specific warnings to be treated as errors, you may specify a comma-separated list of warning numbers to treat as errors. The set of all nullability warnings can be specified with the **nullable** shorthand.
  
 Use [-warn](./warn-compiler-option.md) to specify the level of warnings that you want the compiler to display. Use [-nowarn](./nowarn-compiler-option.md) to disable certain warnings.  
  
### To set this compiler option in the Visual Studio development environment  
  
1. Open the project's **Properties** page.  
  
2. Click the **Build** property page.  
  
3. Modify the **Treat Warnings As Errors** property.  
  
 To set this compiler option programmatically, see <xref:VSLangProj80.CSharpProjectConfigurationProperties3.TreatWarningsAsErrors>.  
  
## Example  
 Compile `in.cs` and have the compiler display no warnings:  
  
```console  
csc -warnaserror in.cs  
csc -warnaserror:642,649,652,nullable in.cs  
```  
  
## See also

- [C# Compiler Options](./index.md)
- [Managing Project and Solution Properties](/visualstudio/ide/managing-project-and-solution-properties)
