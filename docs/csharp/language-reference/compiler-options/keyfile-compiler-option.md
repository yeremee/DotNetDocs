---
description: "-keyfile (C# Compiler Options)"
title: "-keyfile (C# Compiler Options)"
ms.date: 07/20/2015
f1_keywords: 
  - "/keyfile"
helpviewer_keywords: 
  - "/keyfile compiler option [C#]"
  - "-keyfile compiler option [C#]"
  - "keyfile compiler option [C#]"
ms.assetid: 0815f9de-ace4-4e98-b4c6-13c55dea40c2
---
# -keyfile (C# Compiler Options)
Specifies the filename containing the cryptographic key.  
  
## Syntax  
  
```console  
-keyfile:file  
```  
  
## Arguments  
  
|Term|Definition|  
|----------|----------------|  
|`file`|The name of the file containing the strong name key.|  
  
## Remarks  
 When this option is used, the compiler inserts the public key from the specified file into the assembly manifest and then signs the final assembly with the private key. To generate a key file, type sn -k `file` at the command line.  
  
 If you compile with **-target:module**, the name of the key file is held in the module and incorporated into the assembly that is created when you compile an assembly with [-addmodule](./addmodule-compiler-option.md).  
  
 You can also pass your encryption information to the compiler with [-keycontainer](./keycontainer-compiler-option.md). Use [-delaysign](./delaysign-compiler-option.md) if you want a partially signed assembly.  
  
 In case both -keyfile and -keycontainer are specified (either by command line option or by custom attribute) in the same compilation, the compiler will first try the key container. If that succeeds, then the assembly is signed with the information in the key container. If the compiler does not find the key container, it will try the file specified with -keyfile. If that succeeds, the assembly is signed with the information in the key file and the key information will be installed in the key container (similar to sn -i) so that on the next compilation, the key container will be valid.  
  
 Note that a key file might contain only the public key.  
  
 For more information, see [Creating and Using Strong-Named Assemblies](../../../standard/assembly/create-use-strong-named.md) and [Delay Signing an Assembly](../../../standard/assembly/delay-sign.md).  
  
### To set this compiler option in the Visual Studio development environment  
  
1. Open the **Properties** page for the project.  
  
2. Click the **Signing** property page.  
  
3. Modify the **Choose a strong name key file** property.  
  
 You can programmatically access this compiler option with <xref:VSLangProj.ProjectProperties.AssemblyOriginatorKeyFile%2A>.  
  
## See also

- [C# Compiler Options](./index.md)
- [Managing Project and Solution Properties](/visualstudio/ide/managing-project-and-solution-properties)
