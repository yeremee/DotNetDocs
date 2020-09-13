---
description: "-delaysign (C# Compiler Options)"
title: "-delaysign (C# Compiler Options)"
ms.date: 05/15/2018
f1_keywords: 
  - "/delaysign"
helpviewer_keywords: 
  - "-delaysign compiler option [C#]"
  - "delaysign compiler option [C#]"
  - "/delaysign compiler option [C#]"
ms.assetid: bcb058eb-2933-4e7f-b356-5c941db4de75
---
# -delaysign (C# Compiler Options)

This option causes the compiler to reserve space in the output file so that a digital signature can be added later.

## Syntax

```console
-delaysign[ + | - ]
```

## Arguments

`+` &#124; `-`

Use **-delaysign-** if you want a fully signed assembly. Use **-delaysign+** if you only want to place the public key in the assembly. The default is **-delaysign-**.

## Remarks

The **-delaysign** option has no effect unless used with [-keyfile](./keyfile-compiler-option.md) or [-keycontainer](./keycontainer-compiler-option.md).

The **-delaysign** and **-publicsign** options are mutually exclusive.

When you request a fully signed assembly, the compiler hashes the file that contains the manifest (assembly metadata) and signs that hash with the private key. That operation creates a digital signature which is stored in the file that contains the manifest. When an assembly is delay signed, the compiler does not compute and store the signature, but reserves space in the file so the signature can be added later.

For example, using **-delaysign+** allows a tester to put the assembly in the global cache. After testing, you can fully sign the assembly by placing the private key in the assembly using the [Assembly Linker](../../../framework/tools/al-exe-assembly-linker.md) utility.

For more information, see [Creating and Using Strong-Named Assemblies](../../../standard/assembly/create-use-strong-named.md) and [Delay Signing an Assembly](../../../standard/assembly/delay-sign.md).

### To set this compiler option in the Visual Studio development environment

1. Open the **Properties** page for the project.
1. Modify the **Delay sign only** property.

For information on how to set this compiler option programmatically, see <xref:VSLangProj80.ProjectProperties3.DelaySign%2A>.

## See also

- [C# -publicsign option](publicsign-compiler-option.md)
- [C# Compiler Options](index.md)
- [Managing Project and Solution Properties](/visualstudio/ide/managing-project-and-solution-properties)
