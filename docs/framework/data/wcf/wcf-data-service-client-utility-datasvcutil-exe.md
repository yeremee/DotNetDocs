---
title: "WCF Data Service Client Utility (DataSvcUtil.exe)"
ms.date: "03/30/2017"
helpviewer_keywords:
  - "WCF Data Services, generating client data classes"
  - "WCF Data Services, client library"
  - "WCF Data Services, consuming"
ms.assetid: 9d0af606-929b-4c03-b307-3ef5f705afce
---
# WCF Data Service Client Utility (DataSvcUtil.exe)

DataSvcUtil.exe is a command-line tool provided by WCF Data Services that consumes an Open Data Protocol (OData) feed and generates the client data service classes that are needed to access a data service from a .NET Framework client application. This utility can generate data classes by using the following metadata sources:

- The root URI of a data service. The utility requests the service metadata document, which describes the data model exposed by the data service. For more information, see [AtomPub (RFC5023)](https://tools.ietf.org/html/rfc5023#section-8).

- A data model file (.csdl) defined by using the conceptual schema definition language (CSDL), as defined in the [\[MC-CSDL\]: Conceptual Schema Definition File Format](https://docs.microsoft.com/openspecs/windows_protocols/mc-csdl/c03ad8c3-e8b7-4306-af96-a9e52bb3df12) specification.

- An .edmx file created by using the Entity Data Model tools that are provided with the Entity Framework. For more information, see the [\[MC-EDMX\]: Entity Data Model for Data Services Packaging Format](https://docs.microsoft.com/openspecs/windows_protocols/mc-edmx/5dff5e25-56a1-408b-9d44-bff6634c7d16) specification.

For more information, see [How to: Manually Generate Client Data Service Classes](how-to-manually-generate-client-data-service-classes-wcf-data-services.md).

The DataSvcUtil.exe tool is installed in the .NET Framework directory. In many cases, this is located in *C:\Windows\Microsoft.NET\Framework\v4.0*. For 64-bit systems, this is located in *C:\Windows\Microsoft.NET\Framework64\v4.0*. You can also access the DataSvcUtil.exe tool from Developer Command Prompt for Visual Studio.

## Syntax

```console
datasvcutil /out:file [/in:file | /uri:serviceuri] [/dataservicecollection] [/language:devlang] [/nologo] [/version:ver] [/help]
```

## Parameters

|Option|Description|
|------------|-----------------|
|`/dataservicecollection`|Specifies that the code required to bind objects to controls is also generated.|
|`/help`<br /><br /> -or-<br /><br /> `/?`|Displays command syntax and options for the tool.|
|`/in:` *\<file>*|Specifies the .csdl or .edmx file or a directory where the file is located.|
|`/language:`[VB&#124;CSharp]|Specifies the language for the generated source code files. The language defaults to C#.|
|`/nologo`|Suppresses the copyright message from displaying.|
|`/out:` *\<file>*|Specifies the name of the source code file that contains the generated client data service classes.|
|`/uri:` *\<string>*|The URI of the OData feed.|
|`/version:`[1.0&#124;2.0]|Specifies the highest accepted version of OData. The version is determined based on the `DataServiceVersion` attribute of the DataService element in the returned data service metadata. For more information, see [Data Service Versioning](data-service-versioning-wcf-data-services.md). When you specify the `/dataservicecollection` parameter, you must also specify `/version:2.0` to enable data binding.|

## See also

- [Generating the Data Service Client Library](generating-the-data-service-client-library-wcf-data-services.md)
- [How to: Add a Data Service Reference](how-to-add-a-data-service-reference-wcf-data-services.md)
