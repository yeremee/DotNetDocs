---
title: Additions to the csproj format for .NET Core
description: Learn about the differences between existing and .NET Core csproj files
ms.topic: reference
ms.date: 04/08/2019
---

# Additions to the csproj format for .NET Core

This document outlines the changes that were added to the project files as part of the move from *project.json* to *csproj* and [MSBuild](https://github.com/Microsoft/MSBuild). For more information about general project file syntax and reference, see [the MSBuild project file](/visualstudio/msbuild/msbuild-project-file-schema-reference) documentation.

## Implicit package references

Metapackages are implicitly referenced based on the target framework(s) specified in the `<TargetFramework>` or `<TargetFrameworks>` property of your project file. `<TargetFrameworks>` is ignored if `<TargetFramework>` is specified, independent of order.

```xml
 <PropertyGroup>
   <TargetFramework>netcoreapp3.1</TargetFramework>
 </PropertyGroup>
 ```

 ```xml
 <PropertyGroup>
   <TargetFrameworks>netcoreapp3.1;net462</TargetFrameworks>
 </PropertyGroup>
 ```

### Recommendations

Since `Microsoft.NETCore.App` or `NETStandard.Library` metapackages are implicitly referenced, the following are our recommended best practices:

- When targeting .NET Core or .NET Standard, never have an explicit reference to the `Microsoft.NETCore.App` or `NETStandard.Library` metapackages via a `<PackageReference>` item in your project file.
- If you need a specific version of the runtime when targeting .NET Core, you should use the `<RuntimeFrameworkVersion>` property in your project (for example, `1.0.4`) instead of referencing the metapackage.
  - This might happen if you are using [self-contained deployments](../deploying/index.md#publish-self-contained) and you need a specific patch version of 1.0.0 LTS runtime, for example.
- If you need a specific version of the `NETStandard.Library` metapackage when targeting .NET Standard, you can use the `<NetStandardImplicitPackageVersion>` property and set the version you need.
- Don't explicitly add or update references to either the `Microsoft.NETCore.App` or `NETStandard.Library` metapackage in .NET Framework projects. If any version of `NETStandard.Library` is needed when using a .NET Standard-based NuGet package, NuGet automatically installs that version.

## Implicit version for some package references

Most usages of [`<PackageReference>`](#packagereference) require setting the `Version` attribute to specify the NuGet package version to be used. When using .NET Core 2.1 or 2.2 and referencing [Microsoft.AspNetCore.App](/aspnet/core/fundamentals/metapackage-app) or [Microsoft.AspNetCore.All](/aspnet/core/fundamentals/metapackage), however, the  attribute is unnecessary. The .NET Core SDK can automatically select the version of these packages that should be used.

### Recommendation

When referencing the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` packages, do not specify their version. If a version is specified, the SDK may produce warning NETSDK1071. To fix this warning, remove the package version like in the following example:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.App" />
</ItemGroup>
```

> Known issue: the .NET Core 2.1 SDK only supported this syntax when the project also uses Microsoft.NET.Sdk.Web. This is resolved in the .NET Core 2.2 SDK.

These references to ASP.NET Core metapackages have a slightly different behavior from most normal NuGet packages. [Framework-dependent deployments](../deploying/index.md#publish-framework-dependent) of applications that use these metapackages automatically take advantage of the ASP.NET Core shared framework. When you use the metapackages, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application—the ASP.NET Core shared framework contains these assets. The assets in the shared framework are optimized for the target platform to improve application startup time. For more information about shared framework, see [.NET Core distribution packaging](../distribution-packaging.md).

If a version *is* specified, it's treated as the *minimum* version of ASP.NET Core shared framework for framework-dependent deployments and as an *exact* version for self-contained deployments. This can have the following consequences:

- If the version of ASP.NET Core installed on the server is less than the version specified on the PackageReference, the .NET Core process fails to launch. Updates to the metapackage are often available on NuGet.org before updates have been made available in hosting environments such as Azure. Updating the version on the PackageReference to ASP.NET Core could cause a deployed application to fail.
- If the application is deployed as a [self-contained deployment](../deploying/index.md#publish-self-contained), the application may not contain the latest security updates to .NET Core. When a version isn't specified, the SDK can automatically include the newest version of ASP.NET Core in the self-contained deployment.

## Default compilation includes in .NET Core projects

With the move to the *csproj* format in the latest SDK versions, we've moved the default includes and excludes for compile items and embedded resources to the SDK properties files. This means that you no longer need to specify these items in your project file.

The main reason for doing this is to reduce the clutter in your project file. The defaults that are present in the SDK should cover most common use cases, so there is no need to repeat them in every project that you create. This leads to smaller project files that are much easier to understand as well as edit by hand, if needed.

The following table shows which element and which [globs](https://en.wikipedia.org/wiki/Glob_(programming)) are both included and excluded in the SDK:

| Element           | Include glob                              | Exclude glob                                                  | Remove glob              |
|-------------------|-------------------------------------------|---------------------------------------------------------------|----------------------------|
| Compile           | \*\*/\*.cs (or other language extensions) | \*\*/\*.user;  \*\*/\*.\*proj;  \*\*/\*.sln;  \*\*/\*.vssscc  | N/A                      |
| EmbeddedResource  | \*\*/\*.resx                              | \*\*/\*.user; \*\*/\*.\*proj; \*\*/\*.sln; \*\*/\*.vssscc     | N/A                      |
| None              | \*\*/\*                                   | \*\*/\*.user; \*\*/\*.\*proj; \*\*/\*.sln; \*\*/\*.vssscc     | \*\*/\*.cs; \*\*/\*.resx   |

> [!NOTE]
> **Exclude glob** always excludes the `./bin` and `./obj` folders, which are represented by the `$(BaseOutputPath)` and `$(BaseIntermediateOutputPath)` MSBuild properties, respectively. As a whole, all excludes are represented by `$(DefaultItemExcludes)`.

If you have globs in your project and you try to build it using the newest SDK, you'll get the following error:

> Duplicate Compile items were included. The .NET SDK includes Compile items from your project directory by default. You can either remove these items from your project file, or set the 'EnableDefaultCompileItems' property to 'false' if you want to explicitly include them in your project file.

In order to get around this error, you can either remove the explicit `Compile` items that match the ones listed on the previous table, or you can set the `<EnableDefaultCompileItems>` property to `false`, like this:

```xml
<PropertyGroup>
    <EnableDefaultCompileItems>false</EnableDefaultCompileItems>
</PropertyGroup>
```

Setting this property to `false` will disable implicit inclusion, reverting to the behavior of previous SDKs where you had to specify the default globs in your project.

This change does not modify the main mechanics of other includes. However, if you wish to specify, for example, some files to get published with your app, you can still use the known mechanisms in *csproj* for that (for example, the `<Content>` element).

`<EnableDefaultCompileItems>` only disables `Compile` globs but doesn't affect other globs, like the implicit `None` glob, which also applies to \*.cs items. Because of that, **Solution Explorer** will continue show \*.cs items as part of the project, included as `None` items. In a similar way, you can set `<EnableDefaultNoneItems>` to false to disable the implicit `None` glob, like this:

```xml
<PropertyGroup>
    <EnableDefaultNoneItems>false</EnableDefaultNoneItems>
</PropertyGroup>
```

To disable **all implicit globs**, you can set the `<EnableDefaultItems>` property to `false` as in the following example:

```xml
<PropertyGroup>
    <EnableDefaultItems>false</EnableDefaultItems>
</PropertyGroup>
```

## How to see the whole project as MSBuild sees it

While those csproj changes greatly simplify project files, you might want to see the fully expanded project as MSBuild sees it once the SDK and its targets are included. Preprocess the project with [the `/pp` switch](/visualstudio/msbuild/msbuild-command-line-reference#preprocess) of the [`dotnet msbuild`](dotnet-msbuild.md) command, which shows which files are imported, their sources, and their contributions to the build without actually building the project:

```dotnetcli
dotnet msbuild -pp:fullproject.xml
```

If the project has multiple target frameworks, the results of the command should be focused on only one of them by specifying it as an MSBuild property:

```dotnetcli
dotnet msbuild -p:TargetFramework=netcoreapp3.1 -pp:fullproject.xml
```

## Additions

### Sdk attribute

The root `<Project>` element of the *.csproj* file has a new attribute called `Sdk`. `Sdk` specifies which SDK will be used by the project. The SDK, as the [layering document](cli-msbuild-architecture.md) describes, is a set of MSBuild [tasks](/visualstudio/msbuild/msbuild-tasks) and [targets](/visualstudio/msbuild/msbuild-targets) that can build .NET Core code. The following SDKs are available for .NET Core:

1. The .NET Core SDK with the ID of `Microsoft.NET.Sdk`
2. The .NET Core web SDK with the ID of `Microsoft.NET.Sdk.Web`
3. The .NET Core Razor Class Library SDK with the ID of `Microsoft.NET.Sdk.Razor`
4. The .NET Core Worker Service with the ID of `Microsoft.NET.Sdk.Worker` (since .NET Core 3.0)
5. The .NET Core WinForms and WPF with the ID of `Microsoft.NET.Sdk.WindowsDesktop` (since .NET Core 3.0)

You need to have the `Sdk` attribute set to one of those IDs on the `<Project>` element in order to use the .NET Core tools and build your code.

### PackageReference

A `<PackageReference>` item element specifies a [NuGet dependency in the project](/nuget/consume-packages/package-references-in-project-files). The `Include` attribute specifies the package ID.

```xml
<PackageReference Include="package-id" Version="" PrivateAssets="" IncludeAssets="" ExcludeAssets="" />
```

#### Version

The required `Version` attribute specifies the version of the package to restore. The attribute respects the rules of the [NuGet version range](/nuget/concepts/package-versioning#version-ranges) scheme. The default behavior is a minimum version, inclusive match. For example, specifying `Version="1.2.3"` is equivalent to NuGet notation `[1.2.3, )` and means the resolved package will have the version 1.2.3 if available or greater otherwise.

#### IncludeAssets, ExcludeAssets, and PrivateAssets

`IncludeAssets` attribute specifies which assets belonging to the package specified by `<PackageReference>` should be
consumed. By default, all package assets are included.

`ExcludeAssets` attribute specifies which assets belonging to the package specified by `<PackageReference>` should not
be consumed.

`PrivateAssets` attribute specifies which assets belonging to the package specified by `<PackageReference>` should be
consumed but not flow to the next project. The `Analyzers`, `Build` and `ContentFiles` assets are private by default
when this attribute is not present.

> [!NOTE]
> `PrivateAssets` is equivalent to the *project.json*/*xproj* `SuppressParent` element.

These attributes can contain one or more of the following items, separated by the semicolon `;` character if more than
one is listed:

- `Compile` – the contents of the *lib* folder are available to compile against.
- `Runtime` – the contents of the *runtime* folder are distributed.
- `ContentFiles` – the contents of the *contentfiles* folder are used.
- `Build` – the props/targets in the *build* folder are used.
- `Native` – the contents from native assets are copied to the *output* folder for runtime.
- `Analyzers` – the analyzers are used.

Alternatively, the attribute can contain:

- `None` – none of the assets are used.
- `All` – all assets are used.

### DotNetCliToolReference

A `<DotNetCliToolReference>` item element specifies the CLI tool that the user wants to restore in the context of the project. It's a replacement for the `tools` node in *project.json*.

```xml
<DotNetCliToolReference Include="<package-id>" Version="" />
```

Note that `DotNetCliToolReference` is [now deprecated](https://github.com/dotnet/announcements/issues/107) in favor of [.NET Core Local Tools](./global-tools.md#install-a-local-tool).

#### Version

`Version` specifies the version of the package to restore. The attribute respects the rules of the [NuGet versioning](/nuget/create-packages/dependency-versions#version-ranges) scheme. The default behavior is a minimum version, inclusive match. For example, specifying `Version="1.2.3"` is equivalent to NuGet notation `[1.2.3, )` and means the resolved package will have the version 1.2.3 if available or greater otherwise.

### RuntimeIdentifiers

The `<RuntimeIdentifiers>` property element lets you specify a semicolon-delimited list of [Runtime Identifiers (RIDs)](../rid-catalog.md) for the project.
RIDs enable publishing self-contained deployments.

```xml
<RuntimeIdentifiers>win10-x64;osx.10.11-x64;ubuntu.16.04-x64</RuntimeIdentifiers>
```

### RuntimeIdentifier

The `<RuntimeIdentifier>` property element allows you to specify only one [Runtime Identifier (RID)](../rid-catalog.md) for the project. The RID enables publishing a self-contained deployment.

```xml
<RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
```

Use `<RuntimeIdentifiers>` (plural) instead if you need to publish for multiple runtimes. `<RuntimeIdentifier>` can provide faster builds when only a single runtime is required.

### PackageTargetFallback

The `<PackageTargetFallback>` property element allows you to specify a set of compatible targets to be used when restoring packages. It's designed to allow packages that use the dotnet [TxM (Target x Moniker)](/nuget/schema/target-frameworks) to operate with packages that don't declare a dotnet TxM. If your project uses the dotnet TxM, then all the packages it depends on must also have a dotnet TxM, unless you add the `<PackageTargetFallback>` to your project in order to allow non-dotnet platforms to be compatible with dotnet.

The following example provides the fallbacks for all targets in your project:

```xml
<PackageTargetFallback>
    $(PackageTargetFallback);portable-net45+win8+wpa81+wp8
</PackageTargetFallback >
```

The following example specifies the fallbacks only for the `netcoreapp3.1` target:

```xml
<PackageTargetFallback Condition="'$(TargetFramework)'=='netcoreapp3.1'">
    $(PackageTargetFallback);portable-net45+win8+wpa81+wp8
</PackageTargetFallback >
```

## Build events

The way that pre-build and post-build events are specified in the project file has changed. The properties PreBuildEvent and PostBuildEvent are not recommended in the SDK-style project format, because macros such as $(ProjectDir) are not resolved. For example, the following code is no longer supported:

```xml
<PropertyGroup>
    <PreBuildEvent>"$(ProjectDir)PreBuildEvent.bat" "$(ProjectDir)..\" "$(ProjectDir)" "$(TargetDir)"</PreBuildEvent>
</PropertyGroup>
```

In SDK-style projects, use an MSBuild target named `PreBuild` or `PostBuild` and set the `BeforeTargets` property for `PreBuild` or the `AfterTargets` property for `PostBuild`. For the preceding example, use the following code:

```xml
<Target Name="PreBuild" BeforeTargets="PreBuildEvent">
    <Exec Command="&quot;$(ProjectDir)PreBuildEvent.bat&quot; &quot;$(ProjectDir)..\&quot; &quot;$(ProjectDir)&quot; &quot;$(TargetDir)&quot;" />
</Target>

<Target Name="PostBuild" AfterTargets="PostBuildEvent">
   <Exec Command="echo Output written to $(TargetDir)" />
</Target>
```

> [!NOTE]
>You can use any name for the MSBuild targets, but the Visual Studio IDE recognizes `PreBuild` and `PostBuild` targets, so we recommend using those names so that you can edit the commands in the Visual Studio IDE.

## NuGet metadata properties

With the move to MSBuild, we have moved the input metadata that is used when packing a NuGet package from *project.json* to *.csproj* files. The inputs are MSBuild properties so they have to go within a `<PropertyGroup>` group. The following is the list of properties that are used as inputs to the packing process when using the `dotnet pack` command or the `Pack` MSBuild target that is part of the SDK:

### IsPackable

A Boolean value that specifies whether the project can be packed. The default value is `true`.

### PackageVersion

Specifies the version that the resulting package will have. Accepts all forms of NuGet version string. Default is the value of `$(Version)`, that is, of the property `Version` in the project.

### PackageId

Specifies the name for the resulting package. If not specified, the `pack` operation will default to using the `AssemblyName` or directory name as the name of the package.

### Title

A human-friendly title of the package, typically used in UI displays as on nuget.org and the Package Manager in Visual Studio. If not specified, the package ID is used instead.

### Authors

A semicolon-separated list of packages authors, matching the profile names on nuget.org. These are displayed in the NuGet Gallery on nuget.org and are used to cross-reference packages by the same authors.

### PackageDescription

A long description of the package for UI display.

### Description

A long description for the assembly. If `PackageDescription` is not specified, then this property is also used as the description of the package.

### Copyright

Copyright details for the package.

### PackageRequireLicenseAcceptance

A Boolean value that specifies whether the client must prompt the consumer to accept the package license before installing the package. The default is `false`.

### DevelopmentDependency

A Boolean value that specifies whether the package is marked as a development-only dependency, which prevents the package from being included as a dependency in other packages. With PackageReference (NuGet 4.8+), this flag also means that compile-time assets are excluded from compilation. For more information, see [DevelopmentDependency support for PackageReference](https://github.com/NuGet/Home/wiki/DevelopmentDependency-support-for-PackageReference).

### PackageLicenseExpression

An [SPDX license identifier](https://spdx.org/licenses/) or expression. For example, `Apache-2.0`.

Here is the complete list of [SPDX license identifiers](https://spdx.org/licenses/). NuGet.org accepts only OSI or FSF approved licenses when using license type expression.

The exact syntax of the license expressions is described below in [ABNF](https://tools.ietf.org/html/rfc5234).

```abnf
license-id            = <short form license identifier from https://spdx.org/spdx-specification-21-web-version#h.luq9dgcle9mo>

license-exception-id  = <short form license exception identifier from https://spdx.org/spdx-specification-21-web-version#h.ruv3yl8g6czd>

simple-expression = license-id / license-id”+”

compound-expression =  1*1(simple-expression /
                simple-expression "WITH" license-exception-id /
                compound-expression "AND" compound-expression /
                compound-expression "OR" compound-expression ) /
                "(" compound-expression ")" )

license-expression =  1*1(simple-expression / compound-expression / UNLICENSED)
```

> [!NOTE]
> Only one of `PackageLicenseExpression`, `PackageLicenseFile` and `PackageLicenseUrl` can be specified at a time.

### PackageLicenseFile

Path to a license file within the package if you are using a license that hasn’t been assigned an SPDX identifier, or it is a custom license (Otherwise `PackageLicenseExpression` is preferred)

Replaces `PackageLicenseUrl`, can't be combined with `PackageLicenseExpression`, and requires Visual Studio version 15.9.4 and .NET SDK 2.1.502 or 2.2.101 or newer.

You will need to ensure the license file is packed by adding it explicitly to the project, example usage:

```xml
<PropertyGroup>
  <PackageLicenseFile>LICENSE.txt</PackageLicenseFile>
</PropertyGroup>
<ItemGroup>
  <None Include="licenses\LICENSE.txt" Pack="true" PackagePath="$(PackageLicenseFile)"/>
</ItemGroup>
```

### PackageLicenseUrl

A URL to the license that is applicable to the package. (_deprecated since Visual Studio 15.9.4, .NET SDK 2.1.502 and 2.2.101_)

### PackageIcon

A path to an image in the package to use as a package icon. Read more about [`icon` metadata](/nuget/reference/nuspec#icon). [PackageIconUrl is deprecated](/nuget/reference/msbuild-targets#packageiconurl) in favor of PackageIcon.

### PackageReleaseNotes

Release notes for the package.

### PackageTags

A semicolon-delimited list of tags that designates the package.

### PackageOutputPath

Determines the output path in which the packed package will be dropped. Default is `$(OutputPath)`.

### IncludeSymbols
This Boolean value indicates whether the package should create an additional symbols package when the project is packed. The symbols package's format is controlled by the `SymbolPackageFormat` property.

### SymbolPackageFormat
Specifies the format of the symbols package. If "symbols.nupkg", a legacy symbols package will be created with a *.symbols.nupkg* extension containing PDBs, DLLs, and other output files. If "snupkg", a snupkg symbol package will be created containing the portable PDBs. Default is "symbols.nupkg".

### IncludeSource

This Boolean value indicates whether the pack process should create a source package. The source package contains the library's source code as well as PDB files. Source files are put under the `src/ProjectName` directory in the resulting package file.

### IsTool

Specifies whether all output files are copied to the *tools* folder instead of the *lib* folder. This is different from a `DotNetCliTool`, which is specified by setting the `PackageType` in the *.csproj* file.

### RepositoryUrl

Specifies the URL for the repository where the source code for the package resides and/or from which it's being built.

### RepositoryType

Specifies the type of the repository. Default is "git".

### RepositoryBranch
Specifies the name of the source branch in the repository. When the project is packaged in a NuGet package, it's added to the package metadata.

### RepositoryCommit
Optional repository commit or changeset to indicate which source the package was built against. `RepositoryUrl` must also be specified for this property to be included. When the project is packaged in a NuGet package, this commit or changeset is added to the package metadata.

### NoPackageAnalysis

Specifies that pack should not run package analysis after building the package.

### MinClientVersion

Specifies the minimum version of the NuGet client that can install this package, enforced by nuget.exe and the Visual Studio Package Manager.

### IncludeBuildOutput

This Boolean value specifies whether the build output assemblies should be packed into the *.nupkg* file or not.

### IncludeContentInPack

This Boolean value specifies whether any items that have a type of `Content` will be included in the resulting package automatically. The default is `true`.

### BuildOutputTargetFolder

Specifies the folder where to place the output assemblies. The output assemblies (and other output files) are copied into their respective framework folders.

### ContentTargetFolders

This property specifies the default location of where all the content files should go if `PackagePath` is not specified for them. The default value is "content;contentFiles".

### NuspecFile

Relative or absolute path to the *.nuspec* file being used for packing.

> [!NOTE]
> If the *.nuspec* file is specified, it's used **exclusively** for packaging information and any information in the projects is not used.

### NuspecBasePath

Base path for the *.nuspec* file.

### NuspecProperties

Semicolon separated list of key=value pairs.

## AssemblyInfo properties

[Assembly attributes](../../standard/assembly/set-attributes.md) that were typically present in an *AssemblyInfo* file are now automatically generated from properties.

### Properties per attribute

As shown in the following table, each attribute has a property that controls its content and another that disables its generation:

| Attribute                                                      | Property               | Property to disable                             |
|----------------------------------------------------------------|------------------------|-------------------------------------------------|
| <xref:System.Reflection.AssemblyCompanyAttribute>              | `Company`              | `GenerateAssemblyCompanyAttribute`              |
| <xref:System.Reflection.AssemblyConfigurationAttribute>        | `Configuration`        | `GenerateAssemblyConfigurationAttribute`        |
| <xref:System.Reflection.AssemblyCopyrightAttribute>            | `Copyright`            | `GenerateAssemblyCopyrightAttribute`            |
| <xref:System.Reflection.AssemblyDescriptionAttribute>          | `Description`          | `GenerateAssemblyDescriptionAttribute`          |
| <xref:System.Reflection.AssemblyFileVersionAttribute>          | `FileVersion`          | `GenerateAssemblyFileVersionAttribute`          |
| <xref:System.Reflection.AssemblyInformationalVersionAttribute> | `InformationalVersion` | `GenerateAssemblyInformationalVersionAttribute` |
| <xref:System.Reflection.AssemblyProductAttribute>              | `Product`              | `GenerateAssemblyProductAttribute`              |
| <xref:System.Reflection.AssemblyTitleAttribute>                | `AssemblyTitle`        | `GenerateAssemblyTitleAttribute`                |
| <xref:System.Reflection.AssemblyVersionAttribute>              | `AssemblyVersion`      | `GenerateAssemblyVersionAttribute`              |
| <xref:System.Resources.NeutralResourcesLanguageAttribute>      | `NeutralLanguage`      | `GenerateNeutralResourcesLanguageAttribute`     |

Notes:

- `AssemblyVersion` and `FileVersion` default is to take the value of `$(Version)` without suffix. For example, if `$(Version)` is `1.2.3-beta.4`, then the value would be `1.2.3`.
- `InformationalVersion` defaults to the value of `$(Version)`.
- `InformationalVersion` has `$(SourceRevisionId)` appended if the property is present. It can be disabled using `IncludeSourceRevisionInInformationalVersion`.
- `Copyright` and `Description` properties are also used for NuGet metadata.
- `Configuration` is shared with all the build process and set via the `--configuration` parameter of `dotnet` commands.

### GenerateAssemblyInfo

A Boolean that enable or disable all the AssemblyInfo generation. The default value is `true`.

### GeneratedAssemblyInfoFile

The path of the generated assembly info file. Default to a file in the `$(IntermediateOutputPath)` (obj) directory.
