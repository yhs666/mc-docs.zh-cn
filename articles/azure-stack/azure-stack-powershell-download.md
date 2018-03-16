---
title: "从 GitHub 下载 Azure Stack 工具 | Microsoft Docs"
description: "了解如何下载操作 Azure Stack 时所需的工具。"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: E4DF77FA-F468-42B5-B44F-F10ED8049171
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 09/25/2017
ms.date: 03/02/2018
ms.author: v-junlch
ms.openlocfilehash: 1a2f56b31559654998261481a27a04d5dd2eaa1c
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="download-azure-stack-tools-from-github"></a>从 GitHub 下载 Azure Stack 工具

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

**AzureStack-Tools** 是托管 PowerShell 模块的 GitHub 存储库，可用于管理资源并将其部署到 Azure Stack。 如果你打算建立 VPN 连接，则可将这些 PowerShell 模块下载到 Azure Stack 开发工具包或基于 Windows 的外部客户端。 若要获取这些工具，请克隆 GitHub 存储库，或运行以下脚本来下载 **AzureStack-Tools** 文件夹：

```PowerShell
# Change directory to the root directory. 
cd \

# Download the tools archive.
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
cd AzureStack-Tools-master

```

## <a name="functionality-provided-by-the-modules"></a>模块提供的功能

**AzureStack-Tools** 存储库包含支持以下 Azure Stack 功能的 PowerShell 模块：  

| 功能 | 说明 | 谁可以使用此模块？ |
| --- | --- | --- |
| [云功能](user/azure-stack-validate-templates.md) | 使用此模块可获取云的云功能。 例如，可以使用此模块来获取 API 版本和 Azure 资源管理器资源等云功能。 还可以使用此模块获取 Azure Stack 和 Azure 云的 VM 扩展。 | 云操作员和用户 |
| [Azure Stack 计算管理](azure-stack-add-vm-image.md) | 使用此模块可在 Azure Stack Marketplace 中添加或删除 VM 映像。 | 云操作员 |
| [Azure Stack 基础结构管理](https://github.com/Azure/AzureStack-Tools/blob/master/Infrastructure/README.md) | 使用此模块可管理 Azure Stack 基础结构 VM、警报、更新等。 |  云操作员|
| [Azure Stack 的资源管理器策略](user/azure-stack-policy-module.md) | 使用此模块可以配置版本和服务可用性与 Azure Stack 相同的 Azure 订阅或 Azure 资源组。 | 云操作员和用户 |
| [注册到 Azure](azure-stack-register.md) | 使用此模块可将开发工具包实例注册到 Azure。 注册后，可从 Azure 下载 Marketplace 项，并在 Azure Stack 中使用它们。 | 云操作员 |
| [Azure Stack 部署](azure-stack-run-powershell-script.md) | 使用此模块可通过 Azure Stack 虚拟硬盘 (VHD) 映像来准备用于部署和重新部署的 Azure Stack 主计算机。 | 云操作员|
| [连接到 Azure Stack](azure-stack-connect-powershell.md) | 使用此模块可通过 PowerShell 连接到 Azure Stack 实例，并配置与 Azure Stack 的 VPN 连接。 | 云操作员和用户 |
| [Azure Stack 服务管理](azure-stack-create-offer.md) | 使用此模块可创建默认租户产品，该产品通过不同的计算、Azure 存储、网络和 Key Vault 服务提供无限配额。   | 云操作员|
| [模板验证程序](user/azure-stack-validate-templates.md) | 使用此模块可以验证是否可将现有或新的模板部署到 Azure Stack。 | 云操作员和用户|


## <a name="next-steps"></a>后续步骤
- [配置 Azure Stack 用户的 PowerShell 环境](user/azure-stack-powershell-configure-user.md)   
- [通过 VPN 连接到 Azure Stack 开发工具包](azure-stack-connect-azure-stack.md)  

