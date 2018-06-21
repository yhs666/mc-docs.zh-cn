---
title: 在 Azure Stack 中使用命令行部署模板 | Microsoft Docs
description: 了解如何使用跨平台命令行接口 (CLI) 将模板部署到 Azure Stack。
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 9584177f-4af3-4834-864d-930b09ae0995
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/30/2018
ms.date: 04/23/2018
ms.author: v-junlch
ms.reviewer: ''
ms.openlocfilehash: a2529c232c36c2e6d66c5a81457979f5040a8562
ms.sourcegitcommit: 85828a2cbfdb58d3ce05c6ef0bc4a24faf4d247b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2018
ms.locfileid: "31805391"
---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>使用命令行在 Azure Stack 中部署模板

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

使用命令行可将 Azure 资源管理器模板部署到 Azure Stack 开发工具包。 Azure 资源管理器 模板可通过单个协调操作部署和预配应用程序的所有资源。

## <a name="before-you-begin"></a>准备阶段
 - 使用 Azure CLI [安装并连接](azure-stack-version-profiles-azurecli2.md)到 Azure Stack
 - 从[创建存储帐户示例模板](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account)下载文件 *azuredeploy.json* 和 *azuredeploy.parameters.json*。
 
## <a name="deploy-template"></a>部署模板
导航到这些文件下载到的文件夹，并运行以下命令来部署模板：

    azure group create "cliRG" "local" -f azuredeploy.json -d "testDeploy" -e azuredeploy.parameters.json

此命令将模板部署到 Azure Stack POC 默认位置中的资源组 **cliRG**。

## <a name="validate-template-deployment"></a>验证模板部署
若要查看此资源组和存储帐户，请使用以下命令：

    azure group list

    azure storage account list





<!-- Update_Description: link update -->