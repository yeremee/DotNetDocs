---
title: "How to write to a text file - C# Programming Guide"
description: Learn how to write a text file with C#. See several code examples and view additional available resources.
ms.date: 07/20/2015
f1_keywords: 
  - "TextWriter.WriteLine"
  - "StreamWriter.Close"
helpviewer_keywords: 
  - "files [C#], text files"
  - "text, writing to files [C#]"
ms.assetid: 2e99f184-d88b-4719-a7f1-d9ec482aa809
---
# How to write to a text file (C# Programming Guide)
These examples show various ways to write text to a file. The first two examples use static convenience methods on the <xref:System.IO.File?displayProperty=nameWithType> class to write each element of any `IEnumerable<string>` and a string to a text file. Example 3 shows how to add text to a file when you have to process each line individually as you write to the file. Examples 1-3 overwrite all existing content in the file, but example 4 shows you how to append text to an existing file.  
  
 These examples all write string literals to files. If you want to format text written to a file, use the <xref:System.String.Format%2A> method or C# [string interpolation](../../language-reference/tokens/interpolated.md) feature.  
  
## Example  
 [!code-csharp[csFilesandFolders#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#3)]  
  
## Robust Programming  
 The following conditions may cause an exception:  
  
- The file exists and is read-only.  
  
- The path name may be too long.  
  
- The disk may be full.  
  
## See also

- [C# Programming Guide](../index.md)
- [File System and the Registry (C# Programming Guide)](./index.md)
- [Sample: Save a collection to Application Storage](https://code.msdn.microsoft.com/CSWinStoreAppSaveCollection-bed5d6e6)
