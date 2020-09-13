---
title: dotnet sln command
description: The dotnet-sln command provides a convenient option to add, remove, and list projects in a solution file.
ms.date: 02/14/2020
---
# dotnet sln

**This article applies to:** ✔️ .NET Core 2.x SDK and later versions

## Name

`dotnet sln` - Lists or modifies the projects in a .NET Core solution file.

## Synopsis

```dotnetcli
dotnet sln [<SOLUTION_FILE>] [command]

dotnet sln [command] -h|--help
```

## Description

The `dotnet sln` command provides a convenient way to list and modify projects in a solution file.

To use the `dotnet sln` command, the solution file must already exist. If you need to create one, use the [dotnet new](dotnet-new.md) command, as in the following example:

```dotnetcli
dotnet new sln
```

## Arguments

- **`SOLUTION_FILE`**

  The solution file to use. If this argument is omitted, the command searches the current directory for one. If it finds no solution file or multiple solution files, the command fails.

## Options

- **`-h|--help`**

  Prints out a description of how to use the command.

## Commands

### `list`

Lists all projects in a solution file.

#### Synopsis

```dotnetcli
dotnet sln list [-h|--help]
```

#### Arguments

- **`SOLUTION_FILE`**

  The solution file to use. If this argument is omitted, the command searches the current directory for one. If it finds no solution file or multiple solution files, the command fails.

#### Options

- **`-h|--help`**

  Prints out a description of how to use the command.
  
### `add`

Adds one or more projects to the solution file.

#### Synopsis

```dotnetcli
dotnet sln [<SOLUTION_FILE>] add [--in-root] [-s|--solution-folder <PATH>] <PROJECT_PATH> [<PROJECT_PATH>...]
dotnet sln add [-h|--help]
```

#### Arguments

- **`SOLUTION_FILE`**

  The solution file to use. If it is unspecified, the command searches the current directory for one and fails if there are multiple solution files.

- **`PROJECT_PATH`**

  The path to the project or projects to add to the solution. Unix/Linux shell [globbing pattern](https://en.wikipedia.org/wiki/Glob_(programming)) expansions are processed correctly by the `dotnet sln` command.

#### Options

- **`-h|--help`**

  Prints out a description of how to use the command.

- **`--in-root`**

  Places the projects in the root of the solution, rather than creating a solution folder. Available since .NET Core 3.0 SDK.

- **`-s|--solution-folder <PATH>`**

  The destination solution folder path to add the projects to. Available since .NET Core 3.0 SDK.

### `remove`

Removes a project or multiple projects from the solution file.

#### Synopsis

```dotnetcli
dotnet sln [<SOLUTION_FILE>] remove <PROJECT_PATH> [<PROJECT_PATH>...]
dotnet sln [<SOLUTION_FILE>] remove [-h|--help]
```

#### Arguments

- **`SOLUTION_FILE`**

  The solution file to use. If is left unspecified, the command searches the current directory for one and fails if there are multiple solution files.

- **`PROJECT_PATH`**

  The path to the project or projects to add to the solution. Unix/Linux shell [globbing pattern](https://en.wikipedia.org/wiki/Glob_(programming)) expansions are processed correctly by the `dotnet sln` command.

#### Options

- **`-h|--help`**

  Prints out a description of how to use the command.

## Examples

- List the projects in a solution:

  ```dotnetcli
  dotnet sln todo.sln list
  ```

- Add a C# project to a solution:

  ```dotnetcli
  dotnet sln add todo-app/todo-app.csproj
  ```

- Remove a C# project from a solution:

  ```dotnetcli
  dotnet sln remove todo-app/todo-app.csproj
  ```

- Add multiple C# projects to the root of a solution:

  ```dotnetcli
  dotnet sln todo.sln add todo-app/todo-app.csproj back-end/back-end.csproj --in-root
  ```

- Add multiple C# projects to a solution:

  ```dotnetcli
  dotnet sln todo.sln add todo-app/todo-app.csproj back-end/back-end.csproj
  ```

- Remove multiple C# projects from a solution:

  ```dotnetcli
  dotnet sln todo.sln remove todo-app/todo-app.csproj back-end/back-end.csproj
  ```

- Add multiple C# projects to a solution using a globbing pattern (Unix/Linux only):

  ```dotnetcli
  dotnet sln todo.sln add **/*.csproj
  ```

- Add multiple C# projects to a solution using a globbing pattern (Windows PowerShell only):

  ```dotnetcli
  dotnet sln todo.sln add (ls -r **/*.csproj)
  ```

- Remove multiple C# projects from a solution using a globbing pattern (Unix/Linux only):

  ```dotnetcli
  dotnet sln todo.sln remove **/*.csproj
  ```

- Remove multiple C# projects from a solution using a globbing pattern (Windows PowerShell only):

  ```dotnetcli
  dotnet sln todo.sln remove (ls -r **/*.csproj)
  ```
