---
title: "How to write a copy constructor - C# Programming Guide"
description: Learn how to write a copy constructor in C# that takes an instance of class and returns a new instance with the values of the input.
ms.date: 07/20/2015
helpviewer_keywords: 
  - "C# Language, copy constructor"
  - "copy constructor [C#]"
ms.assetid: fba899b5-fc41-428e-a745-3ebdbf37990a
---
# How to write a copy constructor (C# Programming Guide)
C# doesn't provide a copy constructor for objects, but you can write one yourself.  
  
## Example  
 In the following example, the `Person`[class](../../language-reference/keywords/class.md) defines a copy constructor that takes, as its argument, an instance of `Person`. The values of the properties of the argument are assigned to the properties of the new instance of `Person`. The code contains an alternative copy constructor that sends the `Name` and `Age` properties of the instance that you want to copy to the instance constructor of the class.  
  
 [!code-csharp[csProgGuideObjects#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#16)]  
  
## See also

- <xref:System.ICloneable>
- [C# Programming Guide](../index.md)
- [Classes and Structs](./index.md)
- [Constructors](./constructors.md)
- [Finalizers](./destructors.md)
