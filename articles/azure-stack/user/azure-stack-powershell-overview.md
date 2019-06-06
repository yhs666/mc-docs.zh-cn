---
title: Azure Stack 上的 PowerShell | Microsoft Docs
description: Azure Stack 上的 PowerShell 具有一些模块和上下文。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: article
origin.date: 04/25/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: sijuman
ms.lastreviewed: 04/24/2019
ms.openlocfilehash: 4c78fecf765ce25aa35df8143fac70b9b25eb3c9
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66249129"
---
# <a name="get-started-with-powershell-on-azure-stack"></a>Azure Stack 上的 PowerShell 入门

PowerShell 用于从命令行管理资源。 若要生成使用 Azure 资源管理器模型的自动化工具，可以使用 PowerShell。 可将 PowerShell 模块定义为一组 PowerShell 函数，用于对特定的区域进行全方面的管理。 若要使用 Azure Stack，需要能够处理不同的 PowerShell cmdlet 集。

本文将会帮助你熟悉这些在 Azure Stack 上使用的不同 PowerShell 模块。 使用 Azure Stack 上的 PowerShell 时，可与四个 API 集交互。

| API | PowerShell 参考 | REST 参考 |
| --- | --- | --- |
| 1.Azure Resource Manager | [Azure PowerShell 模块](https://github.com/Azure/azure-powershell/blob/master/documentation/azure-powershell-modules.md) | [REST API 浏览器](https://docs.microsoft.com/rest/api/) |
| 2.Azure Stack 资源管理器 | [在 Azure Stack 中管理 API 版本配置文件](azure-stack-version-profiles.md) | [在 Azure Stack 中管理 API 版本配置文件](azure-stack-version-profiles.md) |
| 3.Azure Stack 管理员终结点 | [Azure Stack 管理模块](https://docs.microsoft.com/powershell/azure/azure-stack/overview) | [REST API 浏览器 - Azure Stack](https://docs.microsoft.com/rest/api/?term=Azure%20Azure%20Stack%20Admin) |
| 4.Azure Stack 特权终结点 | [在 Azure Stack 中使用特权终结点](../operator/azure-stack-privileged-endpoint.md) | |

每个接口将会联系 Azure 或 Azure Stack 中的资源提供程序。 资源提供程序启用 Azure 功能。 例如，使用“Azure 计算”资源提供程序可以编程方式访问虚拟机及其支持资源的创建和管理功能。

资源提供程序不仅提供功能，而且还提供用于管理和配置资源的控件。 可以使用 Azure 资源管理器以编程方式访问资源提供程序，接口将提供适用于 PowerShell、Azure CLI 和你自己的 REST 客户端的图面。

## <a name="where-to-find-azure-stack-powershell"></a>在何处可以找到 Azure Stack PowerShell

以下框图显示了每个 PowerShell 模块集的关系。 可以从计算机加载 PowerShell 模块，并管理 Azure 和 Azure Stack。

![Azure Stack Powershell](media/azure-stack-powershell-overview/Azure-Stack-PowerShell.png)

### <a name="azure"></a>Azure

Azure PowerShell 提供一组使用最新版 Azure 资源管理器模型管理 Azure 资源的 cmdlet。 Azure PowerShell 使用了 .NET Standard，这使得它可用于 Windows、macOS 和 Linux。 还可以在 Azure Cloud Shell 中使用 Azure PowerShell。 有关详细信息，请参阅 [Azure PowerShell 入门](https://docs.microsoft.com/powershell/azure/get-started-azureps)。

### <a name="azure-stack-resource-manager"></a>Azure Stack 资源管理器

Azure Stack PowerShell 提供一组 cmdlet，这些 cmdlet 使用与 Azure Stack 中的资源提供程序兼容的旧版 Azure 资源管理器。 Azure Stack 中的每个资源提供程序使用可在全球 Azure 中找到的旧版提供程序。 为了帮助协调 Azure Stack 支持的每个提供程序版本，可以使用 API 配置文件。 Azure Stack PowerShell 使用 PowerShell 5.1，并且只能在 Windows 中使用。 有关详细信息，请参阅[在 Azure Stack 中管理 API 版本配置文件](azure-stack-version-profiles.md)。

### <a name="azure-stack-administrator"></a>Azure Stack 管理员

Azure Stack 向云操作员公开一组资源提供程序用于安装和维护 Azure Stack。 在 Azure 中，这种交互已从用户端抽离，作为 Azure 的一部分在幕后进行处理。 但是，Azure Stack 使企业能够支持私有云。 若要执行这些任务，操作员需要与 Azure Stack 管理 API 交互。 有关详细信息，请参阅[安装适用于 Azure Stack 的 PowerShell](../operator/azure-stack-powershell-install.md)。

### <a name="azure-stack-privileged-endpoint"></a>Azure Stack 特权终结点

对于 Azure Stack 上的操作员活动（例如测试安装和访问日志），操作员可与特权终结点 (PEP) 交互。 PEP 是预先配置的远程 PowerShell 控制台，可为操作员提供刚好足够的访问权限，以帮助他们执行特定的任务。 终结点使用 PowerShell JEA (Just Enough Administration) 公开一组受限的 cmdlet。 有关详细信息，请参阅[使用 Azure Stack 中的特权终结点](../operator/azure-stack-privileged-endpoint.md)。

### <a name="azurestack-tools"></a>AzureStack-Tools

此外，Azure Stack 在 GitHub 存储库上的“Azure Stack 工具”中提供了脚本和其他 cmdlet。 AzureStack-Tools 托管用于管理资源以及将其部署到 Azure Stack 的 PowerShell 模块。 如果你打算建立 VPN 连接，则可将这些 PowerShell 模块下载到 Azure Stack 开发工具包或基于 Windows 的外部客户端。 有关详细信息，请参阅 [AzureStack-Tools](https://github.com/Azure/AzureStack-Tools)。

## <a name="working-with-powershell-on-azure-stack"></a>使用 Azure Stack 上的 PowerShell

PowerShell 提供编程的方式来与 Azure 资源管理器交互。 若要将任务自动化，可以使用交互式命令提示符或编写脚本。

在长时间使用 Azure Stack PowerShell 的过程中，你会发现需要反复安装模块。 如果你同时在使用 Azure，这可能会造成困扰，因为需要根据目标卸载再重新安装模块。 可以使用 Docker 容器来隔离本地计算机上的每个 PowerShell 版本。 [使用 Docker 运行 PowerShell](azure-stack-powershell-user-docker.md) 中介绍了 Docker 容器的用法，使你可以在不同的 PowerShell 模块集之间切换。


## <a name="next-steps"></a>后续步骤

- 了解 Azure Stack 上的 [PowerShell 的 API 配置文件](azure-stack-version-profiles.md)。
- [安装 Azure Stack PowerShell](../operator/azure-stack-powershell-install.md)
- 了解如何创建 [Azure 资源管理器模板](azure-stack-develop-templates.md)以实现云一致性。