---
title: "Delegates - C# Programming Guide"
description: A delegate in C# is a type that refers to methods with a parameter list and return type. Delegates are used to pass methods as arguments to other methods.
ms.date: 07/20/2015
helpviewer_keywords: 
  - "C# language, delegates"
  - "delegates [C#]"
ms.assetid: 97de039b-c76b-4b9c-a27d-8c1e1c8d93da
---
# Delegates (C# Programming Guide)
A [delegate](../../language-reference/builtin-types/reference-types.md) is a type that represents references to methods with a particular parameter list and return type. When you instantiate a delegate, you can associate its instance with any method with a compatible signature and return type. You can invoke (or call) the method through the delegate instance.  
  
 Delegates are used to pass methods as arguments to other methods. Event handlers are nothing more than methods that are invoked through delegates. You create a custom method, and a class such as a windows control can call your method when a certain event occurs. The following example shows a delegate declaration:  
  
 [!code-csharp[csProgGuideDelegates#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#20)]  
  
 Any method from any accessible class or struct that matches the delegate type can be assigned to the delegate. The method can be either static or an instance method. This makes it possible to programmatically change method calls, and also plug new code into existing classes.  
  
> [!NOTE]
> In the context of method overloading, the signature of a method does not include the return value. But in the context of delegates, the signature does include the return value. In other words, a method must have the same return type as the delegate.  
  
 This ability to refer to a method as a parameter makes delegates ideal for defining callback methods. For example, a reference to a method that compares two objects could be passed as an argument to a sort algorithm. Because the comparison code is in a separate procedure, the sort algorithm can be written in a more general way.  
  
## Delegates Overview  
 Delegates have the following properties:  
  
- Delegates are similar to C++ function pointers, but delegates are fully object-oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.
  
- Delegates allow methods to be passed as parameters.  
  
- Delegates can be used to define callback methods.  
  
- Delegates can be chained together; for example, multiple methods can be called on a single event.  
  
- Methods do not have to match the delegate type exactly. For more information, see [Using Variance in Delegates](../concepts/covariance-contravariance/using-variance-in-delegates.md).  
  
- C# version 2.0 introduced the concept of [anonymous methods](../../language-reference/operators/delegate-operator.md), which allow code blocks to be passed as parameters in place of a separately defined method. C# 3.0 introduced lambda expressions as a more concise way of writing inline code blocks. Both anonymous methods and lambda expressions (in certain contexts) are compiled to delegate types. Together, these features are now known as anonymous functions. For more information about lambda expressions, see [Lambda expressions](../../language-reference/operators/lambda-expressions.md).
  
## In This Section  
  
- [Using Delegates](./using-delegates.md)  
  
- [When to Use Delegates Instead of Interfaces (C# Programming Guide)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/ms173173(v=vs.100))  
  
- [Delegates with Named vs. Anonymous Methods](./delegates-with-named-vs-anonymous-methods.md)  
  
- [Using Variance in Delegates](../concepts/covariance-contravariance/using-variance-in-delegates.md)  
  
- [How to combine delegates (Multicast Delegates)](./how-to-combine-delegates-multicast-delegates.md)  
  
- [How to declare, instantiate, and use a delegate](./how-to-declare-instantiate-and-use-a-delegate.md)

## C# Language Specification  

For more information, see [Delegates](~/_csharplang/spec/delegates.md) in the [C# Language Specification](/dotnet/csharp/language-reference/language-specification/introduction). The language specification is the definitive source for C# syntax and usage.
  
## Featured Book Chapters  
 [Delegates, Events, and Lambda Expressions](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff518994%28v=orm.10%29) in [C# 3.0 Cookbook, Third Edition: More than 250 solutions for C# 3.0 programmers](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff518995%28v=orm.10%29)  
  
 [Delegates and Events](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff652490%28v=orm.10%29) in [Learning C# 3.0: Master the fundamentals of C# 3.0](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/ff652493%28v=orm.10%29)  
  
## See also

- <xref:System.Delegate>
- [C# Programming Guide](../index.md)
- [Events](../events/index.md)
