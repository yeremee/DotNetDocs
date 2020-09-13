---
title: Tour of .NET
description: A guided tour through some of the prominent features of .NET.
author: cartermp
ms.author: wiwagn
ms.date: 05/22/2017
ms.technology: dotnet-standard
ms.assetid: bbfe6465-329d-4982-869d-472e7ef85d93
---
# Tour of .NET

.NET is a general purpose development platform. It has several key features, such as support for multiple programming languages, asynchronous and concurrent programming models, and native interoperability, which enable a wide range of scenarios across multiple platforms.

This article offers a guided tour through some of the key features of the .NET. See the [.NET Architectural Components](components.md) topic to learn about the architectural pieces of .NET and what they're used for.

## How to run the code samples

To learn how to set up a development environment to run the code samples, see the [Getting Started](get-started.md) topic. Copy and paste code samples from this page into your environment to execute them.

## Programming languages

.NET supports multiple programming languages. The .NET implementations implement the [Common Language Infrastructure (CLI)](https://visualstudio.microsoft.com/license-terms/ecma-c-common-language-infrastructure-standards/), which among other things specifies a language-independent runtime and language interoperability. This means that you choose any .NET language to build apps and services on .NET.

Microsoft actively develops and supports three .NET languages: C#, F#, and Visual Basic.

* C# is simple, powerful, type-safe, and object-oriented, while retaining the expressiveness and elegance of C-style languages. Anyone familiar with C and similar languages finds few problems in adapting to C#. Check out the [C# Guide](../csharp/index.yml) to learn more about C#.

* F# is a cross-platform, functional-first programming language that also supports traditional object-oriented and imperative programming. Check out the [F# Guide](../fsharp/index.yml) to learn more about F#.

* Visual Basic is an easy language to learn that you use to build a variety of apps that run on .NET. Among the .NET languages, the syntax of Visual Basic is the closest to ordinary human language, often making it easier for people new to software development.

## Automatic memory management

.NET uses [garbage collection (GC)](garbage-collection/index.md) to provide automatic memory management for programs. The GC operates on a lazy approach to memory management, preferring app throughput to the immediate collection of memory. To learn more about the .NET GC, check out [Fundamentals of garbage collection (GC)](garbage-collection/fundamentals.md).

The following two lines both allocate memory:

[!code-csharp[MemoryManagement](../../samples/snippets/csharp/snippets/tour/MemoryManagement.csx#L1-L2)]

There's no analogous keyword to de-allocate memory, as de-allocation happens automatically when the garbage collector reclaims the memory through its scheduled run.

The garbage collector is one of the services that help ensure *memory safety*. A program is memory safe if it accesses only allocated memory. For instance, the runtime ensures that an app doesn't access unallocated memory beyond the bounds of an array.

In the following example, the runtime throws an <xref:System.IndexOutOfRangeException> exception to enforce memory safety:

[!code-csharp[MemoryManagement](../../samples/snippets/csharp/snippets/tour/MemoryManagement.csx#L4-L5)]

## Working with unmanaged resources

Some objects reference *unmanaged resources*. Unmanaged resources are resources that aren't automatically maintained by the .NET runtime. For example, a file handle is an unmanaged resource. A <xref:System.IO.FileStream> object is a managed object, but it references a file handle, which is unmanaged. When you're done using the <xref:System.IO.FileStream>, you need to release the file handle.

In .NET, objects that reference unmanaged resources implement the <xref:System.IDisposable> interface. When you're done using the object, you call the object's <xref:System.IDisposable.Dispose> method, which is responsible for releasing any unmanaged resources. .NET languages provide a convenient [`using` statement](../csharp/language-reference/keywords/using.md) for such objects, as shown in the following example:

[!code-csharp[UnmanagedResources](../../samples/snippets/csharp/snippets/tour/UnmanagedResources.csx#L1-L6)]

Once the `using` block completes, the .NET runtime automatically calls the `stream` object's <xref:System.IDisposable.Dispose> method, which releases the file handle. The runtime also does this if an exception causes control to leave the block.

For more details, see the following topics:

* For C#, see the [using Statement (C# Reference)](../csharp/language-reference/keywords/using-statement.md) topic.
* For F#, see [Resource Management: The use Keyword](../fsharp/language-reference/resource-management-the-use-keyword.md).
* For Visual Basic, see the [Using Statement (Visual Basic)](../visual-basic/language-reference/statements/using-statement.md) topic.

## Type safety

An object is an instance of a specific type. The only operations allowed for a given object are those of its type. A `Dog` type may have `Jump` and `WagTail` methods but not a `SumTotal` method. A program only calls the methods belonging to a given type. All other calls result in either a compile-time error or a run-time exception (in case of using dynamic features or `object`).

.NET languages are object-oriented with hierarchies of base and derived classes. The .NET runtime only allows object casts and calls that align with the object hierarchy. Remember that every type defined in any .NET language derives from the base <xref:System.Object> type.

[!code-csharp[TypeSafety](../../samples/snippets/csharp/snippets/tour/TypeSafety.csx#L19-L23)]

Type safety is also used to help enforce encapsulation by guaranteeing the fidelity of the accessor keywords. Accessor keywords are artifacts which control access to members of a given type by other code. These are usually used for various kinds of data within a type that are used to manage its behavior.

[!code-csharp[TypeSafety](../../samples/snippets/csharp/snippets/tour/TypeSafety.csx#L3-L3)]

C#, Visual Basic, and F# support local *type inference*. Type inference means that the compiler deduces the type of the expression on the left-hand side from the expression on the right-hand side. This doesn't mean that the type safety is broken or avoided. The resulting type does have a strong type with everything that implies. From the previous example, `dog` is rewritten to introduce type inference, and the remainder of the example is unchanged:

[!code-csharp[TypeSafety](../../samples/snippets/csharp/snippets/tour/TypeSafety.csx#L28-L34)]

F# has even further type inference capabilities than the method-local type inference found in C# and Visual Basic. To learn more, see [Type Inference](../fsharp/language-reference/type-inference.md).

## Delegates and lambdas

A delegate is represented by a method signature. Any method with that signature can be assigned to the delegate and is executed when the delegate is invoked.

Delegates are like C++ function pointers except that they're type safe. They're a kind of disconnected method within the CLR type system. Regular methods are attached to a class and are only directly callable through static or instance calling conventions.

In .NET, delegates are commonly used in event handlers, in defining asynchronous operations, and in lambda expressions, which are a cornerstone of LINQ. Learn more in the [Delegates and lambdas](delegates-lambdas.md) topic.

## Generics

Generics allow the programmer to introduce a *type parameter* when designing their classes that allows the client code (the users of the type) to specify the exact type to use in place of the type parameter.

Generics were added to help programmers implement generic data structures. Before their arrival, in order for a type such as the `List` type to be generic, it would have to work with elements that were of type `object`. This had various performance and semantic problems, along with possible subtle run-time errors. A common run-time error is when a data structure contains, for example, both integers and strings, and an <xref:System.InvalidCastException> is thrown while processing the list's members.

The following sample shows a basic program running using an instance of <xref:System.Collections.Generic.List%601> types:

[!code-csharp[GenericsShort](../../samples/snippets/csharp/snippets/tour/GenericsShort.csx)]

For more information, see the [Generic types (Generics) overview](generics.md) topic.

## Async programming

Async programming is a first-class concept within .NET with async support in the runtime, framework libraries, and .NET language constructs. Internally, they're based on objects (such as `Task`), which take advantage of the operating system to perform I/O-bound jobs as efficiently as possible.

To learn more about async programming in .NET, start with the [Async overview](async.md) topic.

## Language Integrated Query (LINQ)

LINQ is a powerful set of features for C# and Visual Basic that allow you to write simple, declarative code for operating on data. The data can be in many forms (such as in-memory objects, a SQL database, or an XML document), but the LINQ code you write typically doesn't differ by data source.

To learn more and see some samples, see the [LINQ (Language Integrated Query) overview](./linq/index.md) article.

## Native interoperability

Every operating system includes an application programming interface (API) that provides system services. .NET provides several ways to call those APIs.

The main way to do native interoperability is via "platform invoke" or P/Invoke for short, which is supported across Linux and Windows platforms. A Windows-only way of doing native interoperability is known as "COM interop," which is used to work with [COM components](/cpp/atl/introduction-to-com) in managed code. It's built on top of the P/Invoke infrastructure, but it works in subtly different ways.

Most of Mono's (and thus Xamarin's) interoperability support for Java and Objective-C are built similarly, that is, they use the same principles.

For more information about native interoperability, see the [Native interoperability](native-interop/index.md) article.

## Unsafe code

Depending on language support, the CLR lets you access native memory and do pointer arithmetic via `unsafe` code. These operations are needed for certain algorithms and system interoperability. Although powerful, use of unsafe code is discouraged unless it's necessary to interop with system APIs or implement the most efficient algorithm. Unsafe code may not execute the same way in different environments and also loses the benefits of a garbage collector and type safety. It's recommended to confine and centralize unsafe code as much as possible and test that code thoroughly.

The following example is a modified version of the `ToString()` method from the `StringBuilder` class. It illustrates how using `unsafe` code can efficiently implement an algorithm by moving around chunks of memory directly:

[!code-csharp[Unsafe](../../samples/snippets/csharp/snippets/tour/Unsafe.csx)]

## Next steps

If you're interested in a tour of C# features, check out [Tour of C#](../csharp/tour-of-csharp/index.md).

If you're interested in a tour of F# features, see [Tour of F#](../fsharp/tour.md).

If you want to get started with writing code of your own, visit [Getting Started](get-started.md).

To learn about important components of .NET, check out [.NET Architectural Components](components.md).
