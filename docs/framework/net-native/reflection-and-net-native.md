---
title: "Reflection and .NET Native"
ms.date: "03/30/2017"
ms.assetid: 91c9eae4-c641-476c-a06e-d7ce39709763
---
# Reflection and .NET Native
In the .NET Framework, managed development supports metaprogramming through the reflection API. Reflection allows you to inspect objects in an app, call methods on objects discovered through inspection, generate new types at run time, and supports many other dynamic code scenarios. It also supports serialization and deserialization, which allows an object's field values to be persisted and later restored. These scenarios all require the .NET Framework just-in-time (JIT) compiler to generate native code based on available metadata.  
  
 The .NET Native runtime doesn't include a JIT compiler. As a result, all necessary native code must be generated in advance. A set of heuristics is used to determine what code should be generated, but these heuristics cannot cover all possible metaprogramming scenarios.  Therefore, you must provide hints for these metaprogramming scenarios by using [runtime directives](runtime-directives-rd-xml-configuration-file-reference.md). If the necessary metadata or implementation code is not available at runtime, your app throws a [MissingMetadataException](missingmetadataexception-class-net-native.md), [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md), or [MissingInteropDataException](missinginteropdataexception-class-net-native.md) exception. Two troubleshooters are available that will generate the appropriate entry for your runtime directives file that eliminates the exception:  
  
- The [MissingMetadataException troubleshooter](https://dotnet.github.io/native/troubleshooter/type.html) for types.  
  
- The [MissingMetadataException troubleshooter](https://dotnet.github.io/native/troubleshooter/method.html) for methods.  
  
> [!NOTE]
> For an overview of the .NET Native compilation process that provides background on why a runtime directives file is needed, see [.NET Native and Compilation](net-native-and-compilation.md).  
  
 In addition, .NET Native doesn't allow you to reflect over private members of the .NET Framework class library. For example, a call to the <xref:System.Reflection.TypeInfo.DeclaredFields%2A?displayProperty=nameWithType> property to retrieve the fields of a .NET Framework class library type returns only public or protected fields.  
  
 The following topics provide the conceptual and reference documentation that you need to support reflection and serialization in your apps:  
  
- [APIs That Rely on Reflection](apis-that-rely-on-reflection.md)  
  
- [Reflection API Reference](net-native-reflection-api-reference.md)  
  
- [Runtime Directives (rd.xml) Configuration File Reference](runtime-directives-rd-xml-configuration-file-reference.md)  
  
## See also

- [Compiling Apps with .NET Native](index.md)
- [.NET Native and Compilation](net-native-and-compilation.md)
