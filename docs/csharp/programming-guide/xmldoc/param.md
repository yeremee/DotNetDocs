---
title: "<param> - C# programming guide"
description: Learn about the XML <param> tag. This tag is used in the comment for a method declaration to describe one of the parameters for the method.
ms.date: 07/20/2015
f1_keywords:
  - "param"
  - "<param>"
helpviewer_keywords:
  - "<param> C# XML tag"
  - "param C# XML tag"
ms.assetid: 46d329b1-5b84-4537-9e17-73ca97313e4e
---
# \<param> (C# programming guide)

## Syntax

```xml
<param name="name">description</param>
```

## Parameters

- `name`

  The name of a method parameter. Enclose the name in double quotation marks (" ").

- `description`

  A description for the parameter.

## Remarks

The `<param>` tag should be used in the comment for a method declaration to describe one of the parameters for the method. To document multiple parameters, use multiple `<param>` tags.

The text for the `<param>` tag is displayed in IntelliSense, the Object Browser, and the Code Comment Web Report.

Compile with [-doc](../../language-reference/compiler-options/doc-compiler-option.md) to process documentation comments to a file.

## Example

[!code-csharp[csProgGuideDocComments#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#1)]

## See also

- [C# programming guide](../index.md)
- [Recommended tags for documentation comments](./recommended-tags-for-documentation-comments.md)
