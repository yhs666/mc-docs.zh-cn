---
title: 将 Azure 资源移动到另一个资源组 | Azure
description: 了解有关如何将 Azure 资源在资源组之间以及订阅之间移动。
services: azure-resource-manager
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
origin.date: 03/04/2019
ms.date: 03/18/2019
ms.topic: tutorial
ms.author: v-yeche
ms.openlocfilehash: fa643738482b39bdf4330578c7f6c20b9dec6022
ms.sourcegitcommit: edce097f471b6e9427718f0641ee2b421e3c0ed2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2019
ms.locfileid: "58348089"
---
<!--Verify successfully-->
# <a name="tutorial-move-azure-resources-to-another-resource-group"></a>教程：将 Azure 资源移动到其他资源组

了解有关如何将 Azure 资源在资源组之间移动。 此外还可以将 Azure 资源在订阅间移动。 在本教程中，使用资源管理器模板来部署两个资源组和一个存储帐户。 然后，将存储帐户从一个资源组移动到另一个资源组。

![“Azure 资源管理器移动资源”示意图](./media/resource-manager-tutorial-move-resources/resource-manager-template-move-resources.png)

本教程涵盖以下任务：

> [!div class="checklist"]
> * 准备资源。
> * 验证该资源可以移动。
> * 移动资源前需查看清单。
> * 验证移动操作。
> * 移动资源。
> * 清理资源。

如果没有 Azure 订阅，请在开始前[创建一个试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="prepare-the-resources"></a>准备资源

创建模板并将其放置到[共享存储帐户](https://armtutorials.blob.core.windows.net/moveresources/azuredeploy.json)。 该模板定义了两个资源组和一个存储帐户。 部署模板时，需要提供项目名称。 项目名称用于生成唯一的资源名称。 

<!--MOONCAKE CUSTOMIZED to meet the Azure China Cloud-->

我们应该下载[共享的存储帐户](https://armtutorials.blob.core.windows.net/moveresources/azuredeploy.json)并在本地电脑上将其另存为 azuredeploy.json 文件。

> [!NOTE]
> 成功下载模板文件 `azuredeploy.json` 后，请将 `"location": "eastus"` 替换为 `"location": "chinaeast"` 并将 `"location": "westus"` 替换为 `"location": "chinanorth"`，使之与 Azure 中国云环境匹配。

修改的变量部分将如下所示：

<!--MOONCAKE CUSTOMIZED to meet the Azure China Cloud-->

```json
"variables": {
  "resourceGroupSource": {
    "name": "[concat(parameters('projectName'), 'rg1')]",
    "location": "chinaeast"
  },
  "resourceGroupDestination": {
    "name": "[concat(parameters('projectName'), 'rg2')]",
    "location": "chinanorth"      
  },
  "storageAccount": {
    "name": "[concat(parameters('projectName'), 'store')]",
    "location": "chinaeast"
  }
},
```

注意 json 中定义的位置，这两个资源组位于中国东部和中国北部。 存储帐户位于中国东部。 将资源移动到具有不同位置的另一个资源组时，移动操作不会更改资源的位置。

<!--Not Available on Select **Try it** to open the Cloud shell, and then execute the PowerShell script inside the Cloud shell:-->

```powershell
$projectName = Read-Host -prompt "Enter a project name"
New-AzDeployment `
    -Name $projectname `
    -Location "chinaeast" `
    -TemplateFile "C:\azuredeploy.json" `
    -projectName $projectName
```

等待脚本成功完成，然后打开 [Azure 门户](https://portal.azure.cn)，并验证资源组和存储帐户是否按预期部署。

> [!NOTE]
> 由于模板定义了两个资源组，因此该部署被视为订阅级别部署。 门户模板部署不支持订阅级别部署。 因此在本教程中使用 Azure PowerShell。 Azure CLI 还支持订阅级别部署。 请参阅[为 Azure 订阅创建资源组和资源](./deploy-to-subscription.md)。

## <a name="verify-the-resource-can-be-moved"></a>验证该资源可以移动

并非所有资源均可移动。 在教程中使用的资源是一个可以移动的存储帐户。 若要验证是否可以移动资源，请参阅[可以移动的服务](./resource-group-move-resources.md#services-that-can-be-moved)。

## <a name="checklist-before-moving-resources"></a>移动资源前需查看的清单

对于本教程，此步骤是可选的，因为其已完成。

移动资源之前需执行的一些重要步骤。 请参阅[移动资源前需查看的清单](./resource-group-move-resources.md#checklist-before-moving-resources)。

## <a name="validate-the-move"></a>验证移动

对于本教程，验证移动是可选的，因为其已完成。

验证移动操作可以测试你的移动方案而无需实际移动资源。 使用此操作来确定移动是否会成功。 有关详细信息，请参阅[验证移动](./resource-group-move-resources.md#validate-move)。

## <a name="move-the-resource"></a>移动资源

存储帐户位于源资源组 (rg1) 内，运行以下 PowerShell 脚本将资源移动到目标资源组 (rg2)。 确保使用的项目名称与部署资源时使用的相同。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

```powershell
$projectName = Read-Host -prompt "Enter a project name"
$resourceGroupSource = $projectName + "rg1"
$resourceGroupDestination = $projectName + "rg2"
$storageAccountName = $projectName + "store"

$storageAccount = Get-AzResource -ResourceGroupName $resourceGroupSource -ResourceName $storageAccountName
Move-AzResource -DestinationResourceGroupName $resourceGroupDestination -ResourceId $storageAccount.ResourceId
```

打开 [Azure 门户](https://portal.azure.cn)，验证存储帐户是否已移至其他资源组，并验证存储帐户位置是否仍为中国东部。

移动资源时，源组和目标组会被锁定，直到移动操作完成。 在完成移动之前，将阻止对资源组执行写入和删除操作。 此锁意味着将无法添加、更新或删除资源组中的资源，但并不意味着资源已被冻结。 例如，如果将 SQL Server 及其数据库移到新的资源组中，使用数据库的应用程序体验不到停机， 仍可读取和写入到数据库。

## <a name="clean-up-resources"></a>清理资源

不再需要 Azure 资源时，请通过删除资源组来清理部署的资源。

1. 在 Azure 门户上的左侧菜单中选择“资源组”。
2. 在“按名称筛选”字段中输入资源组名称。
3. 选择源资源组名称。  
4. 在顶部菜单中选择“删除资源组”。
5. 选择目标资源组名称。  
6. 在顶部菜单中选择“删除资源组”。

## <a name="next-steps"></a>后续步骤

在本教程中，你学习了如何将存储帐户从资源组移动到另一个资源组。 到目前为止，你一直在处理一个存储帐户或多个存储帐户实例。 在下一篇教程中，我们将开发包含多个资源和多个资源类型的模板。 某些资源具有依赖的资源。

> [!div class="nextstepaction"]
> [创建依赖资源](./resource-manager-tutorial-create-templates-with-dependent-resources.md)

<!--Update_Description: wording update -->
