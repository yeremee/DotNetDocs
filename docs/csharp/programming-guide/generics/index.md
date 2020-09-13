---
title: "Generics - C# Programming Guide"
description: Learn about generics. Generic types maximize code reuse, type safety, and performance, and are commonly used to create collection classes.
ms.date: 07/20/2015
helpviewer_keywords: 
  - "C# language, generics"
  - "generics [C#]"
ms.assetid: 75ea8509-a4ea-4e7a-a2b3-cf72482e9282
---
# Generics (C# Programming Guide)

Generics introduce the concept of type parameters to .NET, which make it possible to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code. For example, by using a generic type parameter `T`, you can write a single class that other client code can use without incurring the cost or risk of runtime casts or boxing operations, as shown here:

[!code-csharp[csProgGuideGenerics#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#1)]

Generic classes and methods combine reusability, type safety, and efficiency in a way that their non-generic counterparts cannot. Generics are most frequently used with collections and the methods that operate on them. The <xref:System.Collections.Generic> namespace contains several generic-based collection classes. The non-generic collections, such as <xref:System.Collections.ArrayList> are not recommended and are maintained for compatibility purposes. For more information, see [Generics in .NET](../../../standard/generics/index.md).

Of course, you can also create custom generic types and methods to provide your own generalized solutions and design patterns that are type-safe and efficient. The following code example shows a simple generic linked-list class for demonstration purposes. (In most cases, you should use the <xref:System.Collections.Generic.List%601> class provided by .NET instead of creating your own.) The type parameter `T` is used in several locations where a concrete type would ordinarily be used to indicate the type of the item stored in the list. It is used in the following ways:

- As the type of a method parameter in the `AddHead` method.
- As the return type of the `Data` property in the nested `Node` class.
- As the type of the private member `data` in the nested class.

 Note that `T` is available to the nested `Node` class. When `GenericList<T>` is instantiated with a concrete type, for example as a `GenericList<int>`, each occurrence of `T` will be replaced with `int`.

[!code-csharp[csProgGuideGenerics#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#2)]

The following code example shows how client code uses the generic `GenericList<T>` class to create a list of integers. Simply by changing the type argument, the following code could easily be modified to create lists of strings or any other custom type:

[!code-csharp[csProgGuideGenerics#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#3)]

## Generics overview

- Use generic types to maximize code reuse, type safety, and performance.
- The most common use of generics is to create collection classes.
- The .NET class library contains several generic collection classes in the <xref:System.Collections.Generic> namespace. These should be used whenever possible instead of classes such as <xref:System.Collections.ArrayList> in the <xref:System.Collections> namespace.
- You can create your own generic interfaces, classes, methods, events, and delegates.
- Generic classes may be constrained to enable access to methods on particular data types.
- Information on the types that are used in a generic data type may be obtained at run-time by using reflection.

## Related sections

- [Generic Type Parameters](generic-type-parameters.md)
- [Constraints on Type Parameters](constraints-on-type-parameters.md)
- [Generic Classes](generic-classes.md)
- [Generic Interfaces](generic-interfaces.md)
- [Generic Methods](generic-methods.md)
- [Generic Delegates](generic-delegates.md)
- [Differences Between C++ Templates and C# Generics](differences-between-cpp-templates-and-csharp-generics.md)
- [Generics and Reflection](generics-and-reflection.md)
- [Generics in the Run Time](generics-in-the-run-time.md)

## C# language specification

For more information, see the [C# Language Specification](~/_csharplang/spec/types.md#constructed-types).

## See also

- <xref:System.Collections.Generic>
- [C# Programming Guide](../index.md)
- [Types](../types/index.md)
- [\<typeparam>](../xmldoc/typeparam.md)
- [\<typeparamref>](../xmldoc/typeparamref.md)
- [Generics in .NET](../../../standard/generics/index.md)
