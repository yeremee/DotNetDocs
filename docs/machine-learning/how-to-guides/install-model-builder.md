---
title: How to install Model Builder
description: Learn how to install the ML.NET Model Builder tool
author: luisquintanilla
ms.author: luquinta
ms.date: 11/21/2019
ms.custom: mvc, how-to, mlnet-tooling
#Customer intent: As a non-developer I want know how to install Model Builder to add machine learning to my .NET application.
---

# How to install ML.NET Model Builder

Learn how to install ML.NET Model Builder to add machine learning to your .NET applications.

> [!NOTE]
> Model Builder is currently in Preview.

## Prerequisites

- Visual Studio 2017 version 15.9.12 or later / Visual Studio 2019
- .NET Core 2.1 SDK or later.

> [!NOTE]
> .NET Core 3.0 SDK is not currently supported.

## Limitations

- ML.NET Model Builder Extension currently only works on Visual Studio on Windows.
- Training dataset limit of 1GB
- SQL Server has a limit of 100 thousand rows for training
- Microsoft SQL Server Data Tools for Visual Studio 2017 is not supported

## Install

ML.NET Model builder can be installed either through the Visual Studio Marketplace or from within Visual Studio.

### Visual Studio Marketplace

1. Download from [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=MLNET.07)
1. Follow prompts to install onto respective Visual Studio version

### Visual Studio 2017

1. In the menu bar, select **Tools** > **Extensions and Updates**

    ![VS2017 open extensions manager dialog](./media/install-model-builder/vs2017-open-extensions-manager.png)

1. Inside the *Extension and Updates* prompt, select the *Online* node.
1. In the search bar, search for *ML.NET Model Builder* and from the results, select ML.NET Model Builder (Preview)

    ![VS2017 search and install Model Builder extension in extensions manager dialog](./media/install-model-builder/vs2017-install-model-builder.png)

1. Follow the prompts to complete the installation

### Visual Studio 2019

1. On the menu bar, select **Extensions** > **Manage Extensions**

    ![VS2019 open extensions manager dialog](./media/install-model-builder/vs2019-open-extensions-manager.png)

1. Inside the *Extension and Updates* prompt, select the *Online* node.
1. Type *ML.NET Model Builder* into the search bar select ML.NET Model Builder (Preview)

    ![VS2019 search and install Model Builder extension in extensions manager dialog](./media/install-model-builder/vs2019-install-model-builder.png)

1. Follow the prompts to complete the installation

## Uninstall

### Visual Studio 2017

1. On the menu bar, select **Tools** > **Extensions and Updates**

    ![VS2017 open manage extensions dialog](./media/install-model-builder/vs2017-open-extensions-manager.png)

1. Inside the *Extension and Updates* prompt, expand the *Installed* node and select *Tools*
1. Select ML.NET Model Builder (Preview) from the list of tools and then, select *Uninstall*

    ![VS2017 search and uninstall Model Builder extension in extensions manager dialog](./media/install-model-builder/vs2017-uninstall-model-builder.png)

1. Follow the prompts to complete the uninstallation.

### Visual Studio 2019

1. On the menu bar, select **Extensions** > **Manage Extensions**

    ![VS2019 open manage extensions dialog](./media/install-model-builder/vs2019-open-extensions-manager.png)

1. Inside the *Extension and Updates* prompt, expand the *Installed* node and select *Tools*
1. Select ML.NET Model Builder (Preview) from the list of tools and then, select *Uninstall*

    ![VS2019 search and uninstall Model Builder extension in extensions manager dialog](./media/install-model-builder/vs2019-uninstall-model-builder.png)

1. Follow the prompts to complete the uninstallation.

## Upgrade

The upgrade process is similar to the installation process. Either download the latest version from Visual Studio Marketplace or use the Extensions Manager in Visual Studio.
