---
title: .NET architectural components
description: Describes .NET architectural components such as the .NET Standard, .NET implementations, .NET runtimes, and tooling.
author: cartermp
ms.date: 08/23/2017
ms.technology: dotnet-standard
---
# .NET architectural components

A .NET app is developed for and runs in one or more *implementations of .NET*.  Implementations of .NET include the .NET Framework, .NET Core, and Mono. There is an API specification common to all implementations of .NET that's called the .NET Standard. This article gives a brief introduction to each of these concepts.

## .NET Standard

.NET Standard is a set of APIs that are implemented by the Base Class Library of a .NET implementation. More formally, it's a specification of .NET APIs that make up a uniform set of contracts that you compile your code against. These contracts are implemented in each .NET implementation. This enables portability across different .NET implementations, effectively allowing your code to run everywhere.

.NET Standard is also a [target framework](glossary.md#target-framework). If your code targets a version of .NET Standard, it can run on any .NET implementation that supports that version of .NET Standard.

To learn more about .NET Standard and how to target it, see [.NET Standard](net-standard.md).

## .NET implementations

Each implementation of .NET includes the following components:

- One or more runtimes. Examples: CLR for .NET Framework, CoreCLR and CoreRT for .NET Core.
- A class library that implements the .NET Standard and may implement additional APIs. Examples: .NET Framework Base Class Library, .NET Core Base Class Library.
- Optionally, one or more application frameworks. Examples: [ASP.NET](https://www.asp.net/), [Windows Forms](../framework/winforms/windows-forms-overview.md), and [Windows Presentation Foundation (WPF)](../framework/wpf/index.md) are included in the .NET Framework and .NET Core.
- Optionally, development tools. Some development tools are shared among multiple implementations.

There are four primary .NET implementations that Microsoft actively develops and maintains: .NET Core, .NET Framework, Mono, and UWP.

### .NET Core

.NET Core is a cross-platform implementation of .NET and designed to handle server and cloud workloads at scale. It runs on Windows, macOS, and Linux. It implements the .NET Standard, so code that targets the .NET Standard can run on .NET Core. [ASP.NET Core](https://dotnet.microsoft.com/learn/aspnet/what-is-aspnet-core), [Windows Forms](../framework/winforms/windows-forms-overview.md), and [Windows Presentation Foundation (WPF)](../framework/wpf/index.md) all run on .NET Core.

To learn more about .NET Core, see the [.NET Core introduction](../core/introduction.md) and [Choosing between .NET Core and .NET Framework for server apps](choosing-core-framework-server.md).

### .NET Framework

.NET Framework is the original .NET implementation that has existed since 2002. Versions 4.5 and later implement .NET Standard, so code that targets .NET Standard can run on those versions of .NET Framework. It contains additional Windows-specific APIs, such as APIs for Windows desktop development with Windows Forms and WPF. .NET Framework is optimized for building Windows desktop applications.

To learn more about .NET Framework, see the [.NET Framework Guide](../framework/index.yml).

### Mono

Mono is a .NET implementation that is mainly used when a small runtime is required. It is the runtime that powers Xamarin applications on Android, macOS, iOS, tvOS, and watchOS and is focused primarily on a small footprint. Mono also powers games built using the Unity engine.

It supports all of the currently published .NET Standard versions.

Historically, Mono implemented the larger API of the .NET Framework and emulated some of the most popular capabilities on Unix. It is sometimes used to run .NET applications that rely on those capabilities on Unix.

Mono is typically used with a just-in-time compiler, but it also features a full static compiler (ahead-of-time compilation) that is used on platforms like iOS.

To learn more about Mono, see the [Mono documentation](https://www.mono-project.com/docs/).

### Universal Windows Platform (UWP)

UWP is an implementation of .NET that is used for building modern, touch-enabled Windows applications and software for the Internet of Things (IoT). It's designed to unify the different types of devices that you may want to target, including PCs, tablets, phones, and even the Xbox. UWP provides many services, such as a centralized app store, an execution environment (AppContainer), and a set of Windows APIs to use instead of Win32 (WinRT). Apps can be written in C++, C#, Visual Basic, and JavaScript. When using C# and Visual Basic, the .NET APIs are provided by .NET Core.

To learn more about UWP, see [Intro to the Universal Windows Platform](/windows/uwp/get-started/universal-application-platform-guide).

## .NET runtimes

A runtime is the execution environment for a managed program. The OS is part of the runtime environment but is not part of the .NET runtime. Here are some examples of .NET runtimes:

- Common Language Runtime (CLR) for the .NET Framework
- Core Common Language Runtime (CoreCLR) for .NET Core
- .NET Native for Universal Windows Platform
- The Mono runtime for Xamarin.iOS, Xamarin.Android, Xamarin.Mac, and the Mono desktop framework

## .NET tooling and common infrastructure

You have access to an extensive set of tools and infrastructure components that work with every implementation of .NET. These tools and components include:

- The .NET languages and their compilers
- The .NET project system (based on *.csproj*, *.vbproj*, and *.fsproj* files)
- [MSBuild](/visualstudio/msbuild/msbuild), the build engine used to build projects
- [NuGet](/nuget/), Microsoft's package manager for .NET
- Open-source build orchestration tools, such as [CAKE](https://cakebuild.net/) and [FAKE](https://fake.build/)

## Applicable standards

The C# Language and the Common Language Infrastructure (CLI) specifications are standardized through [Ecma International&reg;](https://www.ecma-international.org/). The first editions of these standards were published by Ecma in December 2001.

Subsequent revisions to the standards have been developed by the TC49-TG2 (C#) and TC49-TG3 (CLI) task groups within the Programming Languages Technical Committee ([TC49](https://www.ecma-international.org/memento/tc49.htm)), and adopted by the Ecma General Assembly and subsequently by ISO/IEC JTC 1 via the ISO Fast-Track process.

### Latest standards

The following official Ecma documents are available for [C#](http://www.ecma-international.org/publications/standards/Ecma-334.htm) and the [CLI](http://www.ecma-international.org/publications/standards/Ecma-335.htm) ([TR-84](http://www.ecma-international.org/publications/techreports/E-TR-084.htm)):

- **The C# Language Standard (version 5.0)**: [ECMA-334.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-334.pdf)
- **The Common Language Infrastructure**: Available in [pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf) form and [zip](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.zip) form.
- **Information Derived from the Partition IV XML File**: Available in [pdf](https://www.ecma-international.org/publications/files/ECMA-TR/ECMA%20TR-084.pdf) and [zip](https://www.ecma-international.org/publications/files/ECMA-TR/TR-084.zip) formats.

The official ISO/IEC documents are available from the ISO/IEC [Publicly Available Standards](https://standards.iso.org/ittf/PubliclyAvailableStandards/) page. These links are direct from that page:

- **Information technology - Programming languages - C#**: [ISO/IEC 23270:2018](https://standards.iso.org/ittf/PubliclyAvailableStandards/c075178_ISO_IEC_23270_2018.zip)
- **Information technology — Common Language Infrastructure (CLI) Partitions I to VI**: [ISO/IEC 23271:2012](https://standards.iso.org/ittf/PubliclyAvailableStandards/c058046_ISO_IEC_23271_2012(E).zip)
- **Information technology — Common Language Infrastructure (CLI) — Technical Report on Information Derived from Partition IV XML File**: [ISO/IEC TR 23272:2011](https://standards.iso.org/ittf/PubliclyAvailableStandards/c057955_ISO_IEC_TR_23272_2011.zip)

## See also

- [Choosing between .NET Core and .NET Framework for server apps](choosing-core-framework-server.md)
- [.NET Standard introduction](net-standard.md)
- [.NET Core introduction](../core/introduction.md)
- [.NET Framework Guide](../framework/index.yml)
- [C# Guide](../csharp/index.yml)
- [F# Guide](../fsharp/index.yml)
- [Visual Basic Guide](../visual-basic/index.yml)
