---
title: "How to concatenate multiple strings (C# Guide)"
description: There are multiple ways to concatenate strings in C#. Learn the options and the reasons behind different choices.
ms.date: 02/20/2018
helpviewer_keywords: 
  - "joining strings [C#]"
  - "concatenating strings [C#]"
  - "strings [C#], concatenation"
ms.assetid: 8e16736f-4096-4f3f-be0f-9d4c3ff63520
---
# How to concatenate multiple strings (C# Guide)

*Concatenation* is the process of appending one string to the end of another string. You concatenate strings by using the `+` operator. For string literals and string constants, concatenation occurs at compile time; no run-time concatenation occurs. For string variables, concatenation occurs only at run time.

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

The following example uses concatenation to split a long string literal into smaller strings in order to improve readability in the source code. These parts are concatenated into a single string at compile time. There is no run-time performance cost regardless of the number of strings involved.

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet1":::

To concatenate string variables, you can use the `+` or `+=` operators, [string interpolation](../language-reference/tokens/interpolated.md) or the <xref:System.String.Format%2A?displayProperty=nameWithType>, <xref:System.String.Concat%2A?displayProperty=nameWithType>, <xref:System.String.Join%2A?displayProperty=nameWithType> or <xref:System.Text.StringBuilder.Append%2A?displayProperty=nameWithType> methods. The `+` operator is easy to use and makes for intuitive code. Even if you use several `+` operators in one statement, the string content is copied only once. The following code shows examples of using the `+` and `+=` operators to concatenate strings:

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet2":::

In some expressions, it's easier to concatenate strings using string interpolation, as the following code shows:

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet3":::

> [!NOTE]
> In string concatenation operations, the C# compiler treats a null string the same as an empty string.

Other method to concatenate strings is <xref:System.String.Format%2A?displayProperty=nameWithType>. This method works well when you are building a string from a small number of component strings.

In other cases, you may be combining strings in a loop where you don't know how many source strings you're combining, and the actual number of source strings may be large. The <xref:System.Text.StringBuilder> class was designed for these scenarios. The following code uses the <xref:System.Text.StringBuilder.Append%2A> method of the <xref:System.Text.StringBuilder> class to concatenate strings.

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet4":::

You can read more about the [reasons to choose string concatenation or the `StringBuilder` class](https://docs.microsoft.com/dotnet/api/system.text.stringbuilder#the-string-and-stringbuilder-types).

Another option to join strings from a collection is to use <xref:System.String.Concat%2A?displayProperty=nameWithType> method. Use <xref:System.String.Join%2A?displayProperty=nameWithType> method if source strings should be separated by a delimiter. The following code combines an array of words using both methods:

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet5":::

At last, you can use [LINQ](../programming-guide/concepts/linq/index.md)
and the <xref:System.Linq.Enumerable.Aggregate%2A?displayProperty=nameWithType> method to join strings from a collection. This method combines
the source strings using a lambda expression. The lambda expression does the
work to add each string to the existing accumulation. The following example
combines an array of words by adding a space between each word in the array:

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/Concatenate.cs" id="Snippet6":::

## See also

- <xref:System.String>
- <xref:System.Text.StringBuilder>
- [C# programming guide](../programming-guide/index.md)
- [Strings](../programming-guide/strings/index.md)
