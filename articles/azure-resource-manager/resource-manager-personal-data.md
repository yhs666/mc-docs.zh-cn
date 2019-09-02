---
title: Azure 资源管理器个人数据 | Azure
description: 了解如何管理与 Azure 资源管理器操作相关联的个人数据。
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: conceptual
origin.date: 05/14/2018
ms.date: 07/22/2019
ms.author: v-yeche
ms.openlocfilehash: 77f3ba59db1449d92bb5a56bfaa0cf94acaed682
ms.sourcegitcommit: 5fea6210f7456215f75a9b093393390d47c3c78d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68337402"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>管理与 Azure 资源管理器相关联的个人数据

若要避免公开敏感信息，请删除在部署、资源组或标记中提供的任何个人信息。 Azure 资源管理器提供相关操作，可以让你管理在部署、资源组或标记中提供的个人数据。

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="delete-personal-data-in-deployment-history"></a>删除部署历史记录中的个人数据

进行部署时，资源管理器会在部署历史记录中保留参数值和状态消息。 这些值会一直保留到你从历史记录中删除部署。 若要查看你是否在这些值中提供了个人数据，请列出部署。 如果找到个人数据，请从历史记录中删除部署。

若要在历史记录中列出**部署**，请使用：

* [按资源组列表](https://docs.microsoft.com/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/Get-AzResourceGroupDeployment)
* [az group deployment list](https://docs.azure.cn/zh-cn/cli/group/deployment?view=azure-cli-latest#az-group-deployment-list)

若要从历史记录中删除**部署**，请使用：

* [删除](https://docs.microsoft.com/rest/api/resources/deployments/delete)
* [Remove-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/Remove-AzResourceGroupDeployment)
* [az group deployment delete](https://docs.azure.cn/zh-cn/cli/group/deployment?view=azure-cli-latest#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>在资源组名称中删除个人数据

资源组的名称会一直保留到你删除该资源组。 若要查看你是否在名称中提供了个人数据，请列出资源组。 如果找到个人数据，请[移动资源](resource-group-move-resources.md)至新的资源组，然后删除名称中包含个人数据的资源组。

若要列出**资源组**，请使用：

* [列表](https://docs.microsoft.com/rest/api/resources/resourcegroups/list)
* [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/Get-AzResourceGroup)
* [az group list](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-list)

若要删除**资源组**，请使用：

* [删除](https://docs.microsoft.com/rest/api/resources/resourcegroups/delete)
* [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/Remove-AzResourceGroup)
* [az group delete](https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>删除标记中的个人数据

标记名称和值会一直保留到你删除或修改该标记。 若要查看你是否在标记中提供了个人数据，请列出标记。 如果找到个人数据，请删除这些标记。

若要列出**标记**，请使用：

* [列表](https://docs.microsoft.com/rest/api/resources/tags/list)
* [Get-AzTag](https://docs.microsoft.com/powershell/module/az.resources/Get-AzTag)
* [az tag list](https://docs.azure.cn/zh-cn/cli/tag?view=azure-cli-latest#az-tag-list)

若要删除**标记**，请使用：

* [删除](https://docs.microsoft.com/rest/api/resources/tags/delete)
* [Remove-AzTag](https://docs.microsoft.com/powershell/module/az.resources/Remove-AzTag)
* [az tag delete](https://docs.azure.cn/zh-cn/cli/tag?view=azure-cli-latest#az-tag-delete)

## <a name="next-steps"></a>后续步骤
* 有关 Azure 资源管理器的概述，请参阅[什么是资源管理器？](resource-group-overview.md)

<!--Update_Description: update meta properties-->