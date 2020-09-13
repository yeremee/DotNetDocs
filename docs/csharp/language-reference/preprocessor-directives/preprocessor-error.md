---
description: "#error - C# Reference"
title: "#error - C# Reference"
ms.date: 08/24/2020
f1_keywords:
  - "#error"
helpviewer_keywords:
  - "#error directive [C#]"
ms.assetid: f2a7f3af-4cf9-4111-b369-70204d24b26b
---
# #error (C# Reference)

`#error` lets you generate a [CS1029](../compiler-messages/cs1029.md) user-defined error from a specific location in your code. For example:

```csharp
#error Deprecated code in this method.
```

> [!NOTE]
> The compiler treats `#error version` in a special way and reports a compiler error, CS8304, with a message containing the used compiler and language versions.

## Remarks

A common use of `#error` is in a conditional directive.

It is also possible to generate a user-defined warning with [#warning](./preprocessor-warning.md).

## Example

```csharp
// preprocessor_error.cs
// CS1029 expected
#define DEBUG
class MainClass
{
    static void Main()
    {
#if DEBUG
#error DEBUG is defined
#endif
    }
}
```

## See also

- [C# Reference](../index.md)
- [C# Programming Guide](../../programming-guide/index.md)
- [C# Preprocessor Directives](./index.md)
