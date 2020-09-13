---
title: ".NET Native Reflection API Reference"
ms.date: "03/30/2017"
ms.assetid: 0429c049-22a3-4ba1-9cc8-f6ee91e31d9c
---
# .NET Native Reflection API Reference
.NET Native includes three new exception types: [System.Runtime.CompilerServices.MissingInteropDataException](missinginteropdataexception-class-net-native.md), [System.Reflection.MissingMetadataException](missingmetadataexception-class-net-native.md), and [System.Reflection.MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md). Note the following about all three exception types:  
  
 These types are for internal use only.  
 These three exception types are for the use of the .NET Native tool chain only. The exceptions are thrown when the .NET Native tool chain detects missing data that does not allow program execution to continue.  
  
 Do not handle these exceptions in your code.  
 These exceptions indicate either that metadata needed by your application is absent (the [MissingInteropDataException](missinginteropdataexception-class-net-native.md) and [MissingMetadataException](missingmetadataexception-class-net-native.md) exceptions) or that implementation code needed by your application is missing (the [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) exception). You correct these exception conditions by modifying a runtime directives (.rd.xml) file to make the required metadata or implementation code available at runtime. For more information, see [Runtime Directives (rd.xml) Configuration File Reference](runtime-directives-rd-xml-configuration-file-reference.md). Two troubleshooters are available that supply the appropriate entries for your runtime directives file that will eliminate [MissingMetadataException](missingmetadataexception-class-net-native.md) and [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) exceptions:  
  
- The [MissingMetadataException troubleshooter](https://dotnet.github.io/native/troubleshooter/type.html) for types.  
  
- The [MissingMetadataException troubleshooter](https://dotnet.github.io/native/troubleshooter/method.html) for methods.  
  
> [!NOTE]
> This reference documents three exception types that are unique to .NET Native. For reference documentation for the .NET Framework core reflection API, see the <xref:System.Reflection>, <xref:System.Reflection.Context> and <xref:System.Reflection.Emit> namespaces. For reference documentation for the .NET Framework core interop API, see <xref:System.Runtime.InteropServices>.  
  
## System.Reflection namespace  
 The <xref:System.Reflection> namespace contains the core types used for reflection in the .NET Framework. For .NET Native, it also includes two new exception types:  
  
|Class|Description|  
|-----------|-----------------|  
|[MissingMetadataException](missingmetadataexception-class-net-native.md)|The exception that is thrown when reflection is used to retrieve metadata that isn't present.|  
|[MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md)|The exception that is thrown when metadata for a type or type member is available but its implementation has been removed.|  
  
 For documentation about the other types in this namespace, see the <xref:System.Reflection> reference pages in the .NET Framework documentation set.  
  
## System.Runtime.CompilerServices namespace  
 The <xref:System.Runtime.CompilerServices> namespace includes types designed for user by language compilers. For .NET Native, it also includes a new exception type:  
  
|Class|Description|  
|-----------|-----------------|  
|[MissingInteropDataException](missinginteropdataexception-class-net-native.md)|The exception that is thrown when a manual marshaling method is called, but metadata for a type isn't found by static analysis or in a runtime directives file.|  
  
 For documentation about the other types in this namespace, see the <xref:System.Runtime.CompilerServices> reference pages in the .NET Framework documentation set.  
  
## See also

- [MissingInteropDataException Class](missinginteropdataexception-class-net-native.md)
- [MissingMetadataException Class](missingmetadataexception-class-net-native.md)
- [MissingRuntimeArtifactException Class](missingruntimeartifactexception-class-net-native.md)
- [Getting Started](getting-started-with-net-native.md)
