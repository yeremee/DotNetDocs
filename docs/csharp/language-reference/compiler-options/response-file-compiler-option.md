---
description: "@ (C# Compiler Options)"
title: "@ (C# Compiler Options)"
ms.date: 07/20/2015
f1_keywords: 
  - "@"
helpviewer_keywords: 
  - "response files, specifying for compilation [C#]"
  - "@ compiler option"
ms.assetid: dda4fa9f-a02c-400f-8b6a-d58834e13d7f
---
# @ (C# Compiler Options)
The @ option lets you specify a file that contains compiler options and source code files to compile.  
  
## Syntax  
  
```console  
@response_file  
```  
  
## Arguments  
 `response_file`  
 A file that lists compiler options or source code files to compile.  
  
## Remarks  
 The compiler options and source code files will be processed by the compiler just as if they had been specified on the command line.  
  
 To specify more than one response file in a compilation, specify multiple response file options. For example:  
  
```console  
@file1.rsp @file2.rsp  
```  
  
 In a response file, multiple compiler options and source code files can appear on one line. A single compiler option specification must appear on one line (cannot span multiple lines). Response files can have comments that begin with the # symbol.  
  
 Specifying compiler options from within a response file is just like issuing those commands on the command line. See [Building from the Command Line](./how-to-set-environment-variables-for-the-visual-studio-command-line.md) for more information.  
  
 The compiler processes the command options as they are encountered. Therefore, command line arguments can override previously listed options in response files. Conversely, options in a response file will override options listed previously on the command line or in other response files.  
  
 C# provides the csc.rsp file, which is located in the same directory as the csc.exe file. See [-noconfig](./noconfig-compiler-option.md) for more information on csc.rsp.  
  
 This compiler option cannot be set in the Visual Studio development environment, nor can it be changed programmatically.  
  
## Example  
 The following are a few lines from a sample response file:  
  
```console  
# build the first output file  
-target:exe -out:MyExe.exe source1.cs source2.cs  
```  
  
## See also

- [C# Compiler Options](./index.md)
