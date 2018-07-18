---
title: PowerShell 脚本：创建 Azure 通知中心 | Microsoft Docs
description: 此 PowerShell 脚本可创建 Azure 通知中心。
services: data-factory
author: dimazaid
manager: kpiteira
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 04/14/2018
ms.date: 07/06/2018
ms.author: v-junlch
ms.openlocfilehash: f6198dcc42be5bd38806b600b999d33d6bdb0187
ms.sourcegitcommit: e950fe5260c519e05f8c5bbf193a8ef733a6a2d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "37936362"
---
# <a name="use-powershell-to-create-an-azure-notification-hub"></a>使用 PowerShell 创建 Azure 通知中心

此示例 PowerShell 脚本可创建示例 Azure 通知中心。 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>先决条件
- **Azure 订阅** - 如果没有 Azure 订阅，请在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="sample-script"></a>示例脚本

```powershell
# Set appropriate values for these variables
$resourceGroupName = "<Enter a name for the resource group>"
$nhubnamespace = "<Enter a name for the notification hub namespace>"
$location = "chinanorth"

# Create a resource group.
New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

# Create a namespace for the resource group
New-AzureRmNotificationHubsNamespace -ResourceGroup $resourceGroupName -Namespace $nhubnamespace -Location $location

# Create an input JSON file that you use with the New-AzureRmNotificationHub command
$text = '{"name": "MyNotificationHub",  "Location": "chinanorth",  "Properties": {  }}'
$text | Out-File "inputfile2.json"

# Create a notification hub
New-AzureRmNotificationHub -ResourceGroup $resourceGroupName -Namespace $nhubnamespace -InputFile .\inputfile2.json
```

## <a name="clean-up-deployment"></a>清理部署

运行示例脚本后，可以使用以下命令删除资源组以及与其关联的所有资源：

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令： 

| 命令 | 注释 |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzureRmNotificationHubsNamespace](https://docs.microsoft.com/powershell/module//azurerm.notificationhubs/new-azurermnotificationhubsnamespace) | 为通知中心创建命名空间。 |
| [New-AzureRmNotificationHub](https://docs.microsoft.com/powershell/module//azurerm.notificationhubs/new-azurermnotificationhubsnamespace) | 创建通知中心。 |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) | 删除资源组，包括所有嵌套的资源。 |
|||

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/)。

