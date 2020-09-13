---
title: ".NET Framework Support for Windows Store Apps and Windows Runtime"
ms.date: "03/30/2017"
ms.technology: dotnet-standard
helpviewer_keywords:
  - "Windows Store apps, .NET Framework support for"
  - "Windows Runtime, .NET Framework support for"
  - ".NET for Windows Store apps"
  - ".NET Framework, and Windows Store apps"
  - ".NET Framework, and Windows Runtime"
ms.assetid: 6fa7d044-ae12-4c54-b8ee-50915607a565
---
# .NET Framework Support for Windows Store Apps and Windows Runtime

The .NET Framework 4.5 supports a number of software development scenarios with the Windows Runtime. These scenarios fall into three categories:

- Developing Windows 8.x Store apps with XAML controls, as described in [Roadmap for Windows Store apps using C# or Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10)), [How tos (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/br229566(v=win.10)), and [.NET for Windows Store apps overview](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)).

- Developing class libraries to use in the Windows 8.x Store apps that you create with the .NET Framework.

- Developing Windows Runtime Components, packaged in .WinMD files, which can be used by any programming language that supports the Windows Runtime. For example, see [Creating Windows Runtime Components in C# and Visual Basic](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic).

This topic outlines the support that the .NET Framework provides for all three categories, and describes the scenarios for Windows Runtime Components. The first section includes basic information about the relationship between the .NET Framework and the Windows Runtime, and explains some oddities you might encounter in the Help system and the IDE. The [second section](#WindowsRuntimeComponents) discusses scenarios for developing Windows Runtime Components.

## The Basics

The .NET Framework supports the three development scenarios listed earlier by providing .NET for Windows 8.x Store apps, and by supporting the Windows Runtime itself.

- [.NET Framework and Windows Runtime namespaces](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)#net-framework-and-windows-runtime-namespaces) provides a streamlined view of the .NET Framework class libraries and include only the types and members you can use to create Windows 8.x Store apps and Windows Runtime Components.

  - When you use Visual Studio (Visual Studio 2012 or later) to develop a Windows 8.x Store app or a Windows Runtime component, a set of reference assemblies ensures that you see only the relevant types and members.

  - This streamlined API set is simplified further by the removal of features that are duplicated within the .NET Framework or that duplicate Windows Runtime features. For example, it contains only the generic versions of collection types, and the XML document object model is eliminated in favor of the Windows Runtime XML API set.

  - Features that simply wrap the operating system API are also removed, because the Windows Runtime is easy to call from managed code.

  To read more about the .NET for Windows 8.x Store apps, see the [.NET for Windows Store apps overview](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)). To read about the API selection process, see the [.NET for Metro style apps](https://devblogs.microsoft.com/dotnet/net-for-metro-style-apps/) entry in the .NET blog.

- The [Windows Runtime](/uwp/api/) provides the user interface elements for building Windows 8.x Store apps, and provides access to operating system features. Like the .NET Framework, the Windows Runtime has metadata that enables the C# and Visual Basic compilers to use the Windows Runtime the way they use the .NET Framework class libraries. The .NET Framework makes it easier to use the Windows Runtime by hiding some differences:

  - Some differences in programming patterns between the .NET Framework and the Windows Runtime, such as the pattern for adding and removing event handlers, are hidden. You simply use the .NET Framework pattern.

  - Some differences in commonly used types (for example, primitive types and collections) are hidden. You simply use the .NET Framework type, as discussed in [Differences That Are Visible in the IDE](#DifferencesVisibleInIDE), later in this article.

Most of the time, .NET Framework support for the Windows Runtime is transparent. The next section discusses some of the apparent differences between managed code and the Windows Runtime.

<a name="AboutReferenceDocumentation"></a>

### The .NET Framework and the Windows Runtime Reference Documentation

The Windows Runtime and the .NET Framework documentation sets are separate. If you press F1 to display Help on a type or member, reference documentation from the appropriate set is displayed. However, if you browse through the [Windows Runtime reference](/uwp/api/) you might encounter examples that seem puzzling:

- Topics such as the <xref:Windows.Foundation.Collections.IIterable%601> interface don't have declaration syntax for Visual Basic or C#. Instead, a note appears above the syntax section (in this case, ".NET: This interface appears as System.Collections.Generic.IEnumerable\<T>"). This is because the .NET Framework and the Windows Runtime provide similar functionality with different interfaces. In addition, there are behavioral differences: `IIterable` has a `First` method instead of a <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> method to return the enumerator. Instead of forcing you to learn a different way of performing a common task, the .NET Framework supports the Windows Runtime by making your managed code appear to use the type you're familiar with. You won't see the `IIterable` interface in the IDE, and therefore the only way you'll encounter it in the Windows Runtime reference documentation is by browsing through that documentation directly.

- The <xref:Windows.Web.Syndication.SyndicationFeed.%23ctor(System.String,System.String,Windows.Foundation.Uri)> documentation illustrates a closely related issue: Its parameter types appear to be different for different languages. For C# and Visual Basic, the parameter types are <xref:System.String?displayProperty=nameWithType> and <xref:System.Uri?displayProperty=nameWithType>. Again, this is because the .NET Framework has its own `String` and `Uri` types, and for such commonly used types it doesn't make sense to force .NET Framework users to learn a different way of doing things. In the IDE, the .NET Framework hides the corresponding Windows Runtime types.

- In a few cases, such as the <xref:Windows.UI.Xaml.GridLength> structure, the .NET Framework provides a type with the same name but more functionality. For example, a set of constructor and property topics are associated with `GridLength`, but they have syntax blocks only for Visual Basic and C# because the members are available only in managed code. In the Windows Runtime, structures have only fields. The Windows Runtime structure requires a helper class, <xref:Windows.UI.Xaml.GridLengthHelper>, to provide equivalent functionality. You won't see that helper class in the IDE when you're writing managed code.

- In the IDE, Windows Runtime types appear to derive from <xref:System.Object?displayProperty=nameWithType>. They appear to have members inherited from <xref:System.Object>, such as <xref:System.Object.ToString%2A?displayProperty=nameWithType>. These members operate as they would if the types actually inherited from <xref:System.Object>, and Windows Runtime types can be cast to <xref:System.Object>. This functionality is part of the support that the .NET Framework provides for the Windows Runtime. However, if you view the types in the Windows Runtime reference documentation, no such members appear. The documentation for these apparent inherited members is provided by the <xref:System.Object?displayProperty=nameWithType> reference documentation.

<a name="DifferencesVisibleInIDE"></a>

### Differences That Are Visible in the IDE

In more advanced programming scenarios, such as using a Windows Runtime component written in C# to provide the application logic for a Windows 8.x Store app built for Windows using JavaScript, such differences are apparent in the IDE as well as in the documentation. When your component returns an `IDictionary<int, string>` to JavaScript, and you look at it in the JavaScript debugger, you'll see the methods of `IMap<int, string>` because JavaScript uses the Windows Runtime type. Some commonly used collection types that appear differently in the two languages are shown in the following table:

|Windows Runtime type|Corresponding .NET Framework type|
|--------------------------------------------------------------|---------------------------------------|
|`IIterable<T>`|`IEnumerable<T>`|
|`IIterator<T>`|`IEnumerator<T>`|
|`IVector<T>`|`IList<T>`|
|`IVectorView<T>`|`IReadOnlyList<T>`|
|`IMap<K, V>`|`IDictionary<TKey, TValue>`|
|`IMapView<K, V>`|`IReadOnlyDictionary<TKey, TValue>`|
|`IBindableIterable`|`IEnumerable`|
|`IBindableVector`|`IList`|
|`Windows.UI.Xaml.Data.INotifyPropertyChanged`|`System.ComponentModel.INotifyPropertyChanged`|
|`Windows.UI.Xaml.Data.PropertyChangedEventHandler`|`System.ComponentModel.PropertyChangedEventHandler`|
|`Windows.UI.Xaml.Data.PropertyChangedEventArgs`|`System.ComponentModel.PropertyChangedEventArgs`|

In the Windows Runtime, `IMap<K, V>` and `IMapView<K, V>` are iterated using `IKeyValuePair`. When you pass them to managed code, they appear as `IDictionary<TKey, TValue>` and `IReadOnlyDictionary<TKey, TValue>`, so naturally you use `System.Collections.Generic.KeyValuePair<TKey, TValue>` to enumerate them.

The way interfaces appear in managed code affects the way types that implement these interfaces appear. For example, the `PropertySet` class implements `IMap<K, V>`, which appears in managed code as `IDictionary<TKey, TValue>`. `PropertySet` appears as if it implemented `IDictionary<TKey, TValue>` instead of `IMap<K, V>`, so in managed code it appears to have an `Add` method, which behaves like the `Add` method on .NET Framework dictionaries. It doesn't appear to have an `Insert` method.

For more information about using the .NET Framework to create a Windows Runtime component, and a walkthrough that shows how to use such a component with JavaScript, see [Creating Windows Runtime Components in C# and Visual Basic](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic).

### Primitive Types

To enable the natural use of the Windows Runtime in managed code, .NET Framework primitive types appear instead of Windows Runtime primitive types in your code. In the .NET Framework, primitive types like the `Int32` structure have many useful properties and methods, such as the `Int32.TryParse` method. By contrast, primitive types and structures in the Windows Runtime have only fields. When you use primitives in managed code, they appear to be .NET Framework types, and you can use the properties and methods of the .NET Framework types as you normally would. The following list provides a summary:

- For the Windows Runtime primitives `Int32`, `Int64`, `Single`, `Double`, `Boolean`, `String` (an immutable collection of Unicode characters), `Enum`, `UInt32`, `UInt64`, and `Guid`, use the type of the same name in the `System` namespace.

- For `UInt8`, use `System.Byte`.

- For `Char16`, use `System.Char`.

- For the `IInspectable` interface, use `System.Object`.

- For `HRESULT`, use a structure with one `System.Int32` member.

As with interface types, the only time you might see evidence of this representation is when your .NET Framework project is a Windows Runtime component that is used by a Windows 8.x Store app built using JavaScript.

Other basic, commonly used Windows Runtime types that appear in managed code as their .NET Framework equivalents include the `Windows.Foundation.DateTime` structure, which appears in managed code as the <xref:System.DateTimeOffset?displayProperty=nameWithType> structure, and the `Windows.Foundation.TimeSpan` structure, which appears as the <xref:System.TimeSpan?displayProperty=nameWithType> structure.

### Other Differences

In a few cases, the fact that .NET Framework types appear in your code instead of Windows Runtime types requires action on your part. For example, the <xref:Windows.Foundation.Uri?displayProperty=nameWithType> class appears as <xref:System.Uri?displayProperty=nameWithType> in .NET Framework code. <xref:System.Uri?displayProperty=nameWithType> allows a relative URI, but <xref:Windows.Foundation.Uri?displayProperty=nameWithType> requires an absolute URI. Therefore, when you pass a URI to a Windows Runtime method, you must ensure that it's absolute. See [Passing a URI to the Windows Runtime](passing-a-uri-to-the-windows-runtime.md).

<a name="WindowsRuntimeComponents"></a>

## Scenarios for Developing Windows Runtime Components

The scenarios that are supported for managed Windows Runtime Components depend on the following general principles:

- Windows Runtime Components that are built using the .NET Framework have no apparent differences from other Windows Runtimelibraries. For example, if you re-implement a native Windows Runtime component by using managed code, the two components are outwardly indistinguishable. The fact that your component is written in managed code is invisible to the code that uses it, even if that code is itself managed code. However, internally, your component is true managed code and runs on the common language runtime (CLR).

- Components can contain types that implement application logic, Windows 8.x Store UI controls, or both.

  > [!NOTE]
  > It's good practice to separate UI elements from application logic. Also, you can't use Windows 8.x Store UI controls in a Windows 8.x Store app built for Windows using JavaScript and HTML.

- A component can be a project within a Visual Studio solution for a Windows 8.x Store app, or a reusable component that you can add to multiple solutions.

  > [!NOTE]
  > If your component will be used only with C# or Visual Basic, there's no reason to make it a Windows Runtime component. If you make it an ordinary .NET Framework class library instead, you don't have to restrict its public API surface to Windows Runtime types.

- You can release versions of reusable components by using the Windows Runtime <xref:Windows.Foundation.Metadata.VersionAttribute> attribute to identify which types (and which members within a type) were added in different versions.

- The types in your component can derive from Windows Runtime types. Controls can derive from the primitive control types in the <xref:Windows.UI.Xaml.Controls.Primitives> namespace or from more finished controls such as <xref:Windows.UI.Xaml.Controls.Button>.

  > [!IMPORTANT]
  > Starting with Windows 8 and the .NET Framework 4.5, all public types in a managed Windows Runtime component must be sealed. A type in another Windows Runtime component can't derive from them. If you want to provide polymorphic behavior in your component, you can create an interface and implement it in the polymorphic types.

- All parameter and return types on the public types in your component must be Windows Runtime types (including the Windows Runtime types that your component defines).

The following sections provide examples of common scenarios.

### Application Logic for a Windows 8.x Store App with JavaScript

When you develop a Windows 8.x Store app for Windows using JavaScript, you might find that some parts of the application logic perform better in managed code, or are easier to develop. JavaScript can't use .NET Framework class libraries directly, but you can make the class library a .WinMD file. In this scenario, the Windows Runtime component is an integral part of the app, so it doesn't make sense to provide version attributes.

### Reusable Windows 8.x Store UI Controls

You can package a set of related UI controls in a reusable Windows Runtime component. The component can be marketed on its own or used as an element in the apps you create. In this scenario, it makes sense to use the Windows Runtime <xref:Windows.Foundation.Metadata.VersionAttribute> attribute to improve compatibility.

### Reusable Application Logic from Existing .NET Framework Apps

You can package managed code from your existing desktop apps as a standalone Windows Runtime component. This enables you to use the component in Windows 8.x Store apps built using C++ or JavaScript, as well as in Windows 8.x Store apps built using C# or Visual Basic. Versioning is an option if there are multiple reuse scenarios for the code.

## Related Topics

|Title|Description|
|-----------|-----------------|
|[.NET for Windows Store apps overview](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))|Describes the .NET Framework types and members that you can use to create Windows 8.x Store apps and Windows RuntimeComponents. (In the Windows Dev Center.)|
|[Roadmap for Windows Store apps using C# or Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))|Provides key resources to help you get started developing Windows 8.x Store apps by using C# or Visual Basic, including many Quickstart topics, guidelines, and best practices. (In the Windows Dev Center.)|
|[How tos (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/br229566(v=win.10))|Provides key resources to help you get started developing Windows 8.x Store apps by using C# or Visual Basic, including many Quickstart topics, guidelines, and best practices. (In the Windows Dev Center.)|
|[Creating Windows Runtime Components in C# and Visual Basic](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic)|Describes how to create a Windows Runtime component using the .NET Framework, explains how to use it as part of a Windows 8.x Store app built for Windows using JavaScript, and describes how to debug the combination with Visual Studio. (In the Windows Dev Center.)|
|[Windows Runtime reference](/uwp/api/)|The reference documentation for the Windows Runtime. (In the Windows Dev Center.)|
|[Passing a URI to the Windows Runtime](passing-a-uri-to-the-windows-runtime.md)|Describes an issue that can arise when you pass a URI from managed code to the Windows Runtime, and how to avoid it.|
