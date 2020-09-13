---
title: .NET Core migration from project.json
description: Learn how to migrate an older .NET Core project using project.json
ms.date: 07/19/2017
---
# Migrating .NET Core projects from project.json

This document covers the following three migration scenarios for .NET Core projects:

1. [Migration from a valid latest schema of *project.json* to *csproj*](#migration-from-projectjson-to-csproj)
2. [Migration from DNX to csproj](#migration-from-dnx-to-csproj)
3. [Migration from RC3 and previous .NET Core csproj projects to the final format](#migration-from-earlier-net-core-csproj-formats-to-rtm-csproj)

This document is only applicable to older .NET Core projects that use project.json. It does not apply to migrating from .NET Framework to .NET Core.

## Migration from project.json to csproj

Migration from *project.json* to *.csproj* can be done using one of the following methods:

- [Visual Studio](#visual-studio)
- [dotnet migrate command-line tool](#dotnet-migrate)

Both methods use the same underlying engine to migrate the projects, so the results will be the same for both. In most cases, using one of these two ways to migrate the *project.json* to *csproj* is the only thing that is needed, and no further manual editing of the project file is necessary. The resulting *.csproj* file will be named the same as the containing directory name.

### Visual Studio

When you open an *.xproj* file or a solution file that references *.xproj* files in Visual Studio 2017 or Visual Studio 2019 version 16.2 and earlier, the **One-way upgrade** dialog appears. The dialog displays the projects to be migrated. If you open a solution file, all the projects specified in the solution file are listed. Review the list of projects to be migrated and select **OK**.

![One-way upgrade dialog showing the list of projects to be migrated](media/one-way-upgrade.jpg)

Visual Studio migrates the selected projects automatically. When migrating a solution, if you don't choose all projects, the same dialog appears asking you to upgrade the remaining projects from that solution. After the project is migrated, you can see and modify its contents by right-clicking the project in the **Solution Explorer** window and selecting **Edit \<project name>.csproj**.

Files that were migrated (*project.json*, *global.json*, *.xproj*, and solution file) are moved to a *Backup* folder. The migrated solution file is upgraded to Visual Studio 2017 or Visual Studio 2019 and you won't be able to open that solution file in Visual Studio 2015 or earlier versions. A file named *UpgradeLog.htm* that contains a migration report is also saved and opened automatically.

> [!IMPORTANT]
> In Visual Studio 2019 version 16.3 and later, you cannot load or migrate an *.xproj* file. Additionally, Visual Studio 2015 doesn't provide the ability to migrate an *.xproj* file. If you're using one of these Visual Studio versions, either install a suitable version of Visual Studio, or use the command line migration tool that's described next.

### dotnet migrate

In the command-line scenario, you can use the [`dotnet migrate`](../tools/dotnet-migrate.md) command. It migrates a project, a solution, or a set of folders in that order, depending on which ones were found. When you migrate a project, the project and all its dependencies are migrated.

Files that were migrated (*project.json*, *global.json*, and *.xproj*) are moved to a *backup* folder.

> [!NOTE]
> If you are using Visual Studio Code, the `dotnet migrate` command does not modify Visual Studio Code-specific files such as *tasks.json*. These files need to be changed manually.
> This is also true if you are using an editor or Integrated Development Environment (IDE) other than Visual Studio.

See [A mapping between project.json and csproj properties](../tools/project-json-to-csproj.md) for a comparison of *project.json* and *.csproj* formats.

If you get an error:

> No executable found matching command dotnet-migrate

Run `dotnet --version` to see which version you are using. [`dotnet migrate`](../tools/dotnet-migrate.md) was introduced in .NET Core SDK 1.0.0 and removed in version 3.0.100.
You'll get this error if you have a *global.json* file in the current or parent directory, and the `sdk` version it specifies is outside this range.

## Migration from DNX to csproj

If you are still using DNX for .NET Core development, your migration process should be done in two stages:

1. Use the [existing DNX migration guidance](from-dnx.md) to migrate from DNX to project-json enabled CLI.
2. Follow the steps from the previous section to migrate from *project.json* to *.csproj*.

> [!NOTE]
> DNX has become officially deprecated during the Preview 1 release of the .NET Core CLI.

## Migration from earlier .NET Core csproj formats to RTM csproj

The .NET Core csproj format has been changing and evolving with each new pre-release version of the tooling. There is no tool that will migrate your project file from earlier versions of csproj to the latest, so you need to manually edit the project file. The actual steps depend on the version of the project file you are migrating. The following is some guidance to consider based on the changes that happened between versions:

- Remove the tools version property from the `<Project>` element, if it exists.
- Remove the XML namespace (`xmlns`) from the `<Project>` element.
- If it doesn't exist, add the `Sdk` attribute to the `<Project>` element and set it to `Microsoft.NET.Sdk` or `Microsoft.NET.Sdk.Web`. This attribute specifies that the project uses the SDK to be used. `Microsoft.NET.Sdk.Web` is used for web apps.
- Remove the `<Import Project="$(MSBuildExtensionsPath)\$(MSBuildToolsVersion)\Microsoft.Common.props" />` and `<Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />` statements from the top and bottom of the project. These import statements are implied by the SDK, so there is no need for them to be in the project.
- If you have `Microsoft.NETCore.App` or `NETStandard.Library` `<PackageReference>` items in your project, you should remove them. These package references are [implied by the SDK](https://aka.ms/sdkimplicitrefs).
- Remove the `Microsoft.NET.Sdk` `<PackageReference>` element, if it exists. The SDK reference comes through the `Sdk` attribute on the `<Project>` element.
- Remove the [globs](https://en.wikipedia.org/wiki/Glob_(programming)) that are [implied by the SDK](../project-sdk/overview.md#default-compilation-includes). Leaving these globs in your project will cause an error on build because compile items will be duplicated.

After these steps your project should be fully compatible with the RTM .NET Core csproj format.

For examples of before and after the migration from old csproj format to the new one, see the [Updating Visual Studio 2017 RC – .NET Core Tooling improvements](https://devblogs.microsoft.com/dotnet/updating-visual-studio-2017-rc-net-core-tooling-improvements/) article on the .NET blog.

## See also

- [Port, Migrate, and Upgrade Visual Studio Projects](/visualstudio/porting/port-migrate-and-upgrade-visual-studio-projects)
