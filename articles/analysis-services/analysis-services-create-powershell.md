---
title: 快速入门 - 使用 PowerShell 创建 Azure Analysis Services 服务器 | Azure
description: 了解如何使用 PowerShell 创建 Azure Analysis Services 服务器
author: rockboyfor
manager: digimobile
ms.service: azure-analysis-services
ms.topic: quickstart
origin.date: 10/18/2018
ms.date: 01/21/2019
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: 74d6564a05be053b9713352354a0322ac1f7da83
ms.sourcegitcommit: db9c7f1a7bc94d2d280d2f43d107dc67e5f6fa4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2019
ms.locfileid: "54193059"
---
# <a name="quickstart-create-a-server---powershell"></a>快速入门：创建服务器 - PowerShell

本快速入门介绍如何从命令行使用 PowerShell，以便在 Azure 订阅中创建 Azure Analysis Services 服务器。

## <a name="prerequisites"></a>先决条件

- **Azure 订阅**：访问 [Azure 试用版](https://www.azure.cn/pricing/1rmb-trial-full)来创建一个帐户。
- **Azure Active Directory**：订阅必须与 Azure Active Directory 租户相关联，且该目录中必须有一个帐户。 若要了解详细信息，请参阅[身份验证和用户权限](analysis-services-manage-users.md)。
- **Azure PowerShell 模块 4.0 版或更高版本**。 若要查找版本，请运行 ` Get-Module -ListAvailable AzureRM`。 若要进行安装或升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)。

## <a name="import-azurermanalysisservices-module"></a>导入 AzureRm.AnalysisServices 模块

若要在订阅中创建一台服务器，请使用 [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) 组件模块。 将 AzureRm.AnalysisServices 模块加载到 PowerShell 会话中。

```powershell
Import-Module AzureRM.AnalysisServices
```

## <a name="sign-in-to-azure"></a>登录 Azure

使用 [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) 命令登录到 Azure 订阅。 按屏幕说明操作。

```powershell
Connect-AzureRmAccount -Environment AzureChinaCloud
```

## <a name="create-a-resource-group"></a>创建资源组

[Azure 资源组](../azure-resource-manager/resource-group-overview.md)是在其中以组的形式部署和管理 Azure 资源的逻辑容器。 创建服务器时，必须在订阅中指定资源组。 如果还没有资源组，可以使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 命令新建一个。 以下示例在中国北部区域中创建名为 `myResourceGroup` 的资源组。

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "ChinaNorth"
```

## <a name="create-a-server"></a>创建服务器

使用 [New-AzureRmAnalysisServicesServer](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) 命令创建新的服务器。 以下示例在中国北部区域的 myResourceGroup 中的 B0 层创建名为 myServer 的服务器，并指定 philipc@adventureworks.com 为服务器管理员。
<!--Notice: ChinaNorth is valid and -Sku should be B0,B1,S0-S4-->

```powershell
New-AzureRmAnalysisServicesServer -ResourceGroupName "myResourceGroup" -Name "myserver" -Location ChinaNorth -Sku B0 -Administrator "philipc@adventure-works.com"
```
<!--Notice: ServerName should be lower charactor-->
<!--Notice: -Sku should be B0,B1,S0-S4-->

## <a name="clean-up-resources"></a>清理资源

可以使用 [Remove-AzureRmAnalysisServicesServer](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) 命令将服务器从订阅中删除。 若要继续完成本系列的其他快速入门和教程，请勿删除服务器。 以下示例删除在上一步创建的服务器。

```powershell
Remove-AzureRmAnalysisServicesServer -Name "myserver" -ResourceGroupName "myResourceGroup"
```

## <a name="next-steps"></a>后续步骤

本快速入门介绍了如何使用 PowerShell 在 Azure 订阅中创建服务器。 创建服务器后，可以通过配置（可选）的服务器防火墙对其进行保护。 还可以直接从门户将基本示例数据模型添加到服务器。 拥有一个示例模型有助于了解如何配置模型数据库角色和测试客户端连接。 若要了解更多信息，请继续学习有关添加示例模型的教程。

> [!div class="nextstepaction"]
> [快速入门：配置服务器防火墙 - 门户](analysis-services-qs-firewall.md)      
> [!div class="nextstepaction"]
> [教程：将示例模型添加到服务器](analysis-services-create-sample-model.md)

<!--Update_Description: update meta properties, wording update, update link -->