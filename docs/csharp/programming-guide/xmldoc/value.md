---
title: "<value> - C# programming guide"
description: Learn about the XML <value> tag. This tag allows you to describe the value that a property represents.
ms.date: 07/20/2015
f1_keywords:
  - "<value>"
helpviewer_keywords:
  - "<value> C# XML tag"
  - "value C# XML tag"
ms.assetid: 08dbadaf-9ab6-43d9-9493-98e43bed199a
---
# \<value> (C# programming guide)

## Syntax

```xml
<value>property-description</value>
```

## Parameters

- `property-description`

  A description for the property.

## Remarks

The `<value>` tag lets you describe the value that a property represents. When you add a property via code wizard in the Visual Studio .NET development environment, it adds a [\<summary>](./summary.md) tag for the new property. You should then manually add a `<value>` tag to describe the value that the property represents.

Compile with [-doc](../../language-reference/compiler-options/doc-compiler-option.md) to process documentation comments to a file.

## Example

[!code-csharp[csProgGuideDocComments#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#14)]

## See also

- [C# programming guide](../index.md)
- [Recommended tags for documentation comments](./recommended-tags-for-documentation-comments.md)
