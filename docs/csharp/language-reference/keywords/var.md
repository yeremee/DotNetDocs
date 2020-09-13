---
description: "var - C# Reference"
title: "var - C# Reference"
ms.date: 07/20/2015
f1_keywords: 
  - "var"
  - "var_CSharpKeyword"
helpviewer_keywords: 
  - "var keyword [C#]"
ms.assetid: 0777850a-2691-4e3e-927f-0c850f5efe15
---
# var (C# Reference)

Beginning in Visual C# 3.0, variables that are declared at method scope can have an implicit "type" `var`. An implicitly typed local variable is strongly typed just as if you had declared the type yourself, but the compiler determines the type. The following two declarations of `i` are functionally equivalent:

```csharp
var i = 10; // Implicitly typed.
int i = 10; // Explicitly typed.
```

For more information, see [Implicitly Typed Local Variables](../../programming-guide/classes-and-structs/implicitly-typed-local-variables.md) and [Type Relationships in LINQ Query Operations](../../programming-guide/concepts/linq/type-relationships-in-linq-query-operations.md).

> [!IMPORTANT]
> When `var` is used with nullable reference types enabled, it always implies a nullable reference type even if the expression type isn't nullable.

## Example

The following example shows two query expressions. In the first expression, the use of `var` is permitted but is not required, because the type of the query result can be stated explicitly as an `IEnumerable<string>`. However, in the second expression, `var` allows the result to be a collection of anonymous types, and the name of that type is not accessible except to the compiler itself. Use of `var` eliminates the requirement to create a new class for the result. Note that in Example #2, the `foreach` iteration variable `item` must also be implicitly typed.

[!code-csharp[csrefKeywordsTypes#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/keywordsTypes.cs#18)]

## See also

- [C# Reference](../index.md)
- [C# Programming Guide](../../programming-guide/index.md)
- [Implicitly Typed Local Variables](../../programming-guide/classes-and-structs/implicitly-typed-local-variables.md)
