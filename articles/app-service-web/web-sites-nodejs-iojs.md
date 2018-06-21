---
title: 如何将 io.js 与 Azure 应用服务 Web 应用配合使用
description: 了解如何将 Azure 应用服务中的 Web 应用与 io.js 配合使用。
services: app-service\web
documentationcenter: nodejs
author: rmcmurray
manager: erikre
editor: ''
ms.assetid: d6320725-ffcb-4ad7-ba63-fc72fa2f2808
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
origin.date: 08/17/2017
ms.date: 09/04/2017
ms.author: v-yiso
ms.openlocfilehash: d2c0249398f61803afc1b71308218bad8d5609ba
ms.sourcegitcommit: 0f2694b659ec117cee0110f6e8554d96ee3acae8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
ms.locfileid: "21134780"
---
# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>如何将 io.js 与 Azure 应用服务 Web 应用配合使用
与 Joyent 的 Node.js 项目相比，流行的 Node 分叉 [io.js] 具有许多不同的特性，包括更加开放的监管模型、更快的发行周期，和更快地采纳新的和试验性的 JavaScript 功能。

虽然 [Azure 应用服务](./app-service-changes-existing-services.md) Web 应用预装了许多 Node.js 版本，但它还允许使用用户提供的 Node.js 二进制文件。 本文讨论在应用服务 Web 应用上启用 io.js 的两种方法：使用扩展的部署脚本（自动将 Azure 配置为使用最新的可用 io.js 版本），以及手动上传 io.js 二进制文件。 

<a id="deploymentscript"></a>

## <a name="using-a-deployment-script"></a>使用部署脚本
在部署 Node.js 应用后，应用服务 Web 应用将运行大量的小命令来确保正确配置环境。 如果使用部署脚本，则可以将此过程自定义为包含 io.js 的下载和配置。

GitHub 上提供了 [io.js 部署脚本](https://github.com/felixrieseberg/iojs-azure)。 要在 Web 应用上启用 io.js，只需将 **.deployment**、**deploy.cmd** 和 **IISNode.yml** 复制到应用程序文件夹的根目录，并将其部署到 Web 应用。  

第一个文件 **.deployment** 指示 Web 应用在部署后要运行 **deploy.cmd**。 此脚本针对 Node.js 应用程序运行所有常见步骤，但还会下载最新版本的 io.js。 最后，**IISNode.yml** 将 Web 应用配置为使用所下载的 io.js 二进制文件，而不是预装的 Node.js 二进制文件。

> [!NOTE]
> 要更新使用的 io.js 二进制文件，只需重新部署你的应用程序 - 每次部署应用程序后，脚本会下载 io.js 的新版本。
> 
> 

<a id="manualinstallation"></a>

## <a name="using-manual-installation"></a>使用手动安装
手动安装自定义 io.js 版本只包括两个步骤。 首先，直接从 [io.js 分发包]中下载 **win-x64** 二进制文件。 需要两个文件 - **iojs.exe** 和 **iojs.lib**。 将这两个文件保存到 Web 应用内的某个文件夹中，例如，保存在 **bin/iojs** 中。

要将 Web 应用配置为使用 **iojs.exe** 而不是预装的 Node 版本，请在应用程序的根目录中创建一个 **IISNode.yml** 文件，并添加以下行。

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>

## <a name="next-steps"></a>后续步骤
在本文中，已学习了如何使用提供的部署脚本以及手动安装方法，将 io.js 与应用服务 Web 应用配合使用。 

> [!NOTE]
> io.js 正在紧张的开发中，其更新频率超过了 Node.js。 许多 Node.js 模块可能并不适用于 io.js - 若要进行故障排除，请查阅 [GitHub 上的 io.js]。
> 
> 

## <a name="whats-changed"></a>发生的更改
* 有关从网站更改为应用服务的指南，请参阅 [Azure 应用服务及其对现有 Azure 服务的影响](./app-service-changes-existing-services.md)

[io.js]: https://iojs.org
[io.js 分发包]: https://iojs.org/dist/
[GitHub 上的 io.js]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure