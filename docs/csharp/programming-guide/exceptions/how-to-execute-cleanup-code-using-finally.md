---
title: "How to execute cleanup code using finally - C# Programming Guide"
description: Learn how to execute cleanup code using a 'finally' statement. Finally statements ensure that any necessary cleanup of objects occurs immediately.
ms.date: 07/20/2015
helpviewer_keywords: 
  - "try/finally blocks [C#]"
  - "exceptions [C#], try/finally block"
  - "exception handling [C#], try/finally block"
ms.assetid: 1b1e5aef-3f32-4a88-9d39-b5fffb33bdaf
---
# How to execute cleanup code using finally (C# Programming Guide)
The purpose of a `finally` statement is to ensure that the necessary cleanup of objects, usually objects that are holding external resources, occurs immediately, even if an exception is thrown. One example of such cleanup is calling <xref:System.IO.Stream.Close%2A> on a <xref:System.IO.FileStream> immediately after use instead of waiting for the object to be garbage collected by the common language runtime, as follows:  
  
 [!code-csharp[csProgGuideExceptions#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#16)]  
  
## Example  
 To turn the previous code into a `try-catch-finally` statement, the cleanup code is separated from the working code, as follows.  
  
 [!code-csharp[csProgGuideExceptions#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#17)]  
  
 Because an exception can occur at any time within the `try` block before the `OpenWrite()` call, or the `OpenWrite()` call itself could fail, we are not guaranteed that the file is open when we try to close it. The `finally` block adds a check to make sure that the <xref:System.IO.FileStream> object is not `null` before you call the <xref:System.IO.Stream.Close%2A> method. Without the `null` check, the `finally` block could throw its own <xref:System.NullReferenceException>, but throwing exceptions in `finally` blocks should be avoided if it is possible.  
  
 A database connection is another good candidate for being closed in a `finally` block. Because the number of connections allowed to a database server is sometimes limited, you should close database connections as quickly as possible. If an exception is thrown before you can close your connection, this is another case where using the `finally` block is better than waiting for garbage collection.  
  
## See also

- [C# Programming Guide](../index.md)
- [Exceptions and Exception Handling](./index.md)
- [Exception Handling](./exception-handling.md)
- [using Statement](../../language-reference/keywords/using-statement.md)
- [try-catch](../../language-reference/keywords/try-catch.md)
- [try-finally](../../language-reference/keywords/try-finally.md)
- [try-catch-finally](../../language-reference/keywords/try-catch-finally.md)
