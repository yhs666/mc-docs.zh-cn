---
title: 在 Azure Stack 中使用命令行部署模板 | Microsoft Docs
description: 了解如何使用跨平台命令行接口 (CLI) 将模板部署到 Azure Stack。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: article
origin.date: 05/09/2019
ms.date: 06/03/2019
ms.author: v-jay
ms.reviewer: unknown
ms.lastreviewed: 05/09/2019
ms.openlocfilehash: 3c0e3412f2a97ac2aac0e5e533ec70ceb825800e
ms.sourcegitcommit: 77d6ceb6a14a3316a6088859c4d9978115b2454a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248544"
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>使用命令行在 Azure Stack 中部署模板

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

使用 Azure 命令行接口 (CLI) 在 Azure Stack 中部署 Azure 资源管理器模板。 Azure 资源管理器模板可通过单个协调操作部署和预配应用的资源。

## <a name="before-you-begin"></a>准备阶段

- 使用 Azure CLI [安装并连接](azure-stack-version-profiles-azurecli2.md)到 Azure Stack。
- 从[创建存储帐户示例模板](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account)下载文件 *azuredeploy.json* 和 *azuredeploy.parameters.json*。

## <a name="deploy-template"></a>部署模板

导航到这些文件下载到的文件夹，并运行以下命令来部署模板：

```azurecli
az group create "cliRG" "local" -f azuredeploy.json -d "testDeploy" -e azuredeploy.parameters.json
```

此命令将模板部署到 Azure Stack POC 默认位置中的资源组 **cliRG**。

## <a name="validate-template-deployment"></a>验证模板部署

若要查看此资源组和存储帐户，请使用以下 CLI 命令：

```azurecli
az group list

az storage account list
```

## <a name="next-steps"></a>后续步骤

详细了解如何[使用 PowerShell 部署模板](azure-stack-deploy-template-powershell.md)。
