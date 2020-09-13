---
description: "-doc (C# Compiler Options)"
title: "-doc (C# Compiler Options)"
ms.date: 07/20/2015
f1_keywords: 
  - "FileProperties.BuildAction"
  - "/doc"
helpviewer_keywords: 
  - "comments, C# code"
  - "XML documentation [C#], comments in source files"
  - "doc compiler option [C#]"
  - "Visual C#, XML documentation for"
  - "-doc compiler option [C#]"
  - "/doc compiler option [C#]"
ms.assetid: 849eea59-c936-4311-bad8-d07404480f2a
---
# -doc (C# Compiler Options)
The **-doc** option allows you to place documentation comments in an XML file.  
  
## Syntax  
  
```console  
-doc:file  
```  
  
## Arguments  
 `file`  
 The output file for XML, which is populated with the comments in the source code files of the compilation.  
  
## Remarks  
 In source code files, documentation comments that precede the following can be processed and added to the XML file:  
  
- Such user-defined types as a [class](../keywords/class.md), [delegate](../builtin-types/reference-types.md#the-delegate-type), or [interface](../keywords/interface.md)  
  
- Such members as a field, [event](../keywords/event.md), [property](../../programming-guide/classes-and-structs/using-properties.md), or method  
  
 The source code file that contains Main is output first into the XML.  
  
 To use the generated .xml file for use with the [IntelliSense](/visualstudio/ide/using-intellisense) feature, let the file name of the .xml file be the same as the assembly you want to support and then make sure the .xml file is in the same directory as the assembly. Thus, when the assembly is referenced in the Visual Studio project, the .xml file is found as well. See [Supplying Code Comments](/visualstudio/ide/reference/generate-xml-documentation-comments) and for more information.  
  
 Unless you compile with [-target:module](./target-module-compiler-option.md), `file` will contain \<assembly>\</assembly> tags specifying the name of the file containing the assembly manifest for the output file of the compilation.  
  
> [!NOTE]
> The -doc option applies to all input files; or, if set in the Project Settings, all files in the project. To disable warnings related to documentation comments for a specific file or section of code, use [#pragma warning](../preprocessor-directives/preprocessor-pragma-warning.md).  
  
 See [Recommended Tags for Documentation Comments](../../programming-guide/xmldoc/recommended-tags-for-documentation-comments.md) for ways to generate documentation from comments in your code.  
  
### To set this compiler option in the Visual Studio development environment  
  
1. Open the project's **Properties** page.  
  
2. Click the **Build** tab.  
  
3. Modify the **XML documentation file** property.  
  
 For information on how to set this compiler option programmatically, see <xref:VSLangProj80.CSharpProjectConfigurationProperties3.DocumentationFile%2A>.  
  
## See also

- [C# Compiler Options](./index.md)
- [Managing Project and Solution Properties](/visualstudio/ide/managing-project-and-solution-properties)
