---
title: 模板示例
description: Azure 资源管理器模板示例，用于部署管理功能，例如角色和锁。
ms.topic: sample
origin.date: 11/16/2018
ms.date: 12/09/2019
ms.openlocfilehash: 7706c79e2b10a7d6da335fc36dead079e0c0a9bc
ms.sourcegitcommit: cf73284534772acbe7a0b985a86a0202bfcc109e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74884903"
---
# <a name="azure-resource-manager-templates-for-management-features"></a>适用于管理功能的 Azure 资源管理器模板

下表包含的 Azure 资源管理器模板链接，此类模板适用于资源管理器提供的功能。

> [!NOTE]
> 必须修改从 GitHub 存储库“azure-quickstart-templates”下载或参考的模板，以适应 Azure 中国云环境。 例如，替换某些终结点（将“blob.core.windows.net”替换为“blob.core.chinacloudapi.cn”，将“cloudapp.azure.com”替换为“chinacloudapp.cn”）；必要时更改某些不受支持的位置、VM 映像、VM 大小、SKU 以及资源提供程序的 API 版本。
> 

| | |
|-|-|
|**角色分配**||
| [为资源组分配角色](https://github.com/Azure/azure-quickstart-templates/tree/master/101-rbac-builtinrole-resourcegroup)| 将内置角色分配到现有资源组的用户。 |
| [为现有虚拟机分配角色](https://github.com/Azure/azure-quickstart-templates/tree/master/101-rbac-builtinrole-virtualmachine)| 将内置角色分配到现有 VM 的用户。 |
| [为多个虚拟机分配角色](https://github.com/Azure/azure-quickstart-templates/tree/master/201-rbac-builtinrole-multipleVMs)| 将内置角色分配到多个虚拟机的用户。 |
| [为 Azure 订阅分配角色](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/subscription-role-assigment)| 为 Azure 订阅的用户分配角色。 |
|**角色定义**||
| [创建自定义角色定义](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-role-def)| 在 Azure 订阅中创建新的角色定义。 |
|**资源锁**||
| [锁定资源组](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-rg-lock-role-assignment)| 创建一个资源组，并将 **DoNotDelete** 锁定应用到资源组。 将参与者角色分配给用户。 |
| | |

<!-- Update_Description: update meta properties, wording update, update link -->