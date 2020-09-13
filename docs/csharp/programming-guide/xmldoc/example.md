---
title: "<example> - C# programming guide"
description: Learn about the XML <example> tag. This tag lets you specify an example of how to use a method or other library member.
ms.date: 07/20/2015
f1_keywords:
  - "<example>"
  - "example"
helpviewer_keywords:
  - "<example> C# XML tag"
  - "example C# XML tag"
ms.assetid: 32d6e73b-2554-4abb-83ee-a1e321334fd2
---
# \<example> (C# programming guide)

## Syntax

```xml
<example>description</example>
```

## Parameters

- `description`

  A description of the code sample.

## Remarks

The `<example>` tag lets you specify an example of how to use a method or other library member. This commonly involves using the [\<code>](./code.md) tag.

Compile with [-doc](../../language-reference/compiler-options/doc-compiler-option.md) to process documentation comments to a file.

## Example

[!code-csharp[csProgGuideDocComments#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#3)]

## See also

- [C# Programming Guide](../index.md)
- [Recommended tags for documentation comments](./recommended-tags-for-documentation-comments.md)
