---
description: "#region - C# Reference"
title: "#region - C# Reference"
ms.date: 07/20/2015
f1_keywords: 
  - "#region"
helpviewer_keywords: 
  - "#region directive [C#]"
ms.assetid: 672c87d1-9771-4f64-ab3f-0ad3d4ffb2b4
---
# #region (C# Reference)
`#region` lets you specify a block of code that you can expand or collapse when using the [outlining](/visualstudio/ide/outlining) feature of the code editor. In longer code files, it is convenient to be able to collapse or hide one or more regions so that you can focus on the part of the file that you are currently working on. The following example shows how to define a region:  
  
```csharp
#region MyClass definition  
public class MyClass
{  
    static void Main()
    {  
    }  
}  
#endregion  
```  
  
## Remarks  
 A `#region` block must be terminated with a [#endregion](./preprocessor-endregion.md) directive.  
  
 A `#region` block cannot overlap with a [#if](./preprocessor-if.md) block. However, a `#region` block can be nested in a `#if` block, and a `#if` block can be nested in a `#region` block.  
  
## See also

- [C# Reference](../index.md)
- [C# Programming Guide](../../programming-guide/index.md)
- [C# Preprocessor Directives](./index.md)
