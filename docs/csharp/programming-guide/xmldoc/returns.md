---
title: "<returns> - C# programming guide"
description: Learn about the XML <returns> tag. This tag is used in the comment for a method declaration to describe the return value.
ms.date: 07/20/2015
f1_keywords:
  - "returns"
  - "<returns>"
helpviewer_keywords:
  - "<returns> C# XML tag"
  - "returns C# XML tag"
ms.assetid: bb2d9958-62fc-47c7-9511-6311171f119f
---
# \<returns> (C# programming guide)

## Syntax

```xml
<returns>description</returns>
```

## Parameters

- `description`

  A description of the return value.

## Remarks

The `<returns>` tag should be used in the comment for a method declaration to describe the return value.

Compile with [-doc](../../language-reference/compiler-options/doc-compiler-option.md) to process documentation comments to a file.

## Example

[!code-csharp[csProgGuideDocComments#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#10)]

## See also

- [C# programming guide](../index.md)
- [Recommended tags for documentation comments](./recommended-tags-for-documentation-comments.md)
