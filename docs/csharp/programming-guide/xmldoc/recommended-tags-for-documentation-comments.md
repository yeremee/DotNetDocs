---
title: "Recommended tags for documentation comments - C# programming guide"
description: Learn about the recommended tags for documentation comments. See a list of recommended tags and view additional available resources.
ms.date: 01/21/2020
helpviewer_keywords:
  - "XML [C#], tags"
  - "XML documentation [C#], tags"
ms.assetid: 6e98f7a9-38f4-4d74-b644-1ff1b23320fd
---
# Recommended tags for documentation comments (C# programming guide)

The C# compiler processes documentation comments in your code and formats them as XML in a file whose name you specify in the **/doc** command-line option. To create the final documentation based on the compiler-generated file, you can create a custom tool, or use a tool such as [DocFX](https://dotnet.github.io/docfx/) or [Sandcastle](https://github.com/EWSoftware/SHFB).

Tags are processed on code constructs such as types and type members.

> [!NOTE]
> Documentation comments cannot be applied to a namespace.  
  
 The compiler will process any tag that is valid XML. The following tags provide generally used functionality in user documentation.  
  
## Tags  
  
|||||  
|---|---|---|---|
|[\<c>](./code-inline.md)|[\<para>](./para.md)|[\<see>](./see.md)*|[\<value>](./value.md)  
|[\<code>](./code.md)|[\<param>](./param.md)*|[\<seealso>](./seealso.md)*|  
|[\<example>](./example.md)|[\<paramref>](./paramref.md)|[\<summary>](./summary.md)|  
|[\<exception>](./exception.md)*|[\<permission>](./permission.md)*|[\<typeparam>](./typeparam.md)*|  
|[\<include>](./include.md)*|[\<remarks>](./remarks.md)|[\<typeparamref>](./typeparamref.md)|  
|[\<list>](./list.md)|[\<inheritdoc>](./inheritdoc.md)|[\<returns>](./returns.md)|
  
(\* denotes that the compiler verifies syntax.)

If you want angle brackets to appear in the text of a documentation comment, use the HTML encoding of `<` and `>` which is `&lt;` and `&gt;` respectively. This encoding is shown in the following example.

```csharp
/// <summary>
/// This property always returns a value &lt; 1.
/// </summary>
```

## See also

- [C# programming guide](../index.md)
- [-doc (C# compiler options)](../../language-reference/compiler-options/doc-compiler-option.md)
- [XML documentation comments](./index.md)
