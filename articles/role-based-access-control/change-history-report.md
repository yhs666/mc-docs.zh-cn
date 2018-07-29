---
title: 在 Azure 中查看 RBAC 更改的活动日志 | Microsoft Docs
description: 查看过去 90 天内基于角色的访问控制 (RBAC) 更改的活动日志。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 05/23/2018
ms.date: 07/25/2018
ms.author: v-junlch
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 97c6fc92ae109104fb4d30fd9a24d344886903a4
ms.sourcegitcommit: cce18df2de12353f0d8f01c649307a5789d59cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/25/2018
ms.locfileid: "39246134"
---
# <a name="view-activity-logs-for-rbac-changes"></a>查看 RBAC 更改的活动日志

有时需要了解基于角色的访问控制 (RBAC) 更改，如出于审核或故障排除目的。 只要有人更改了你订阅中的角色分配或角色定义，这些更改就会被记录到 [Azure 活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)中。 可以查看活动日志，了解在过去 90 天内发生的所有 RBAC 更改。

## <a name="operations-that-are-logged"></a>记录的操作

下面是记录在活动日志中的 RBAC 相关操作：

- 创建角色分配
- 删除角色分配
- 创建或更新自定义角色定义
- 删除自定义角色定义

## <a name="azure-portal"></a>Azure 门户

最简单的入手方式就是使用 Azure 门户查看活动日志。 下面的屏幕截图展示了已筛选为显示角色分配和角色定义操作的活动日志示例。 它还包括一个能将日志下载为 CSV 文件的链接。

![使用门户的活动日志 - 屏幕截图](./media/change-history-report/activity-log-portal.png)

门户中的活动日志有多个筛选器。 下面是与 RBAC 相关的筛选器：

|筛选器  |值  |
|---------|---------|
|事件类别     | <ul><li>管理</li></ul>         |
|操作     | <ul><li>创建角色分配</li> <li>删除角色分配</li> <li>创建或更新自定义角色定义</li> <li>删除自定义角色定义</li></ul>      |


若要详细了解活动日志，请参阅[查看活动日志中的事件](/azure-resource-manager/resource-group-audit)。

## <a name="azure-powershell"></a>Azure PowerShell

若要使用 Azure PowerShell 查看活动日志，请使用 [Get-AzureRmLog](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermlog) 命令。

此命令列出过去 7 天内订阅中所有角色分配的更改：

```azurepowershell
Get-AzureRmLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

此命令列出过去 7 天内资源组中所有角色定义的更改：

```azurepowershell
Get-AzureRmLog -ResourceGroupName pharma-sales-projectforecast -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

此命令列出过去 7 天内订阅中所有角色分配和角色定义的更改，并在列表中显示结果：

```azurepowershell
Get-AzureRmLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/role*'} | Format-List Caller,EventTimestamp,{$_.Authorization.Action},Properties
```

```Example
Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:07 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          statusCode     : Created
                          serviceRequestId: 11111111-1111-1111-1111-111111111111

Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:05 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          requestbody    : {"Id":"22222222-2222-2222-2222-222222222222","Properties":{"PrincipalId":"33333333-3333-3333-3333-333333333333","RoleDefinitionId":"/subscriptions/00000000-0000-0000-0000-000000000000/providers
                          /Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","Scope":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast"}}

```

## <a name="azure-cli"></a>Azure CLI

若要使用 Azure CLI 查看活动日志，请使用 [az monitor activity-log list](/cli/monitor/activity-log#az-monitor-activity-log-list) 命令。

此命令列出从启动以来资源组中存在的活动日志：

```azurecli
az monitor activity-log list --resource-group pharma-sales-projectforecast --start-time 2018-04-20T00:00:00Z
```

此命令列出从启动以来授权资源提供程序的活动日志：

```azurecli
az monitor activity-log list --resource-provider "Microsoft.Authorization" --start-time 2018-04-20T00:00:00Z
```

## <a name="next-steps"></a>后续步骤
- [在活动日志中查看事件](/azure-resource-manager/resource-group-audit)
- [使用 Azure 活动日志监视订阅活动](/monitoring-and-diagnostics/monitoring-overview-activity-logs)

<!-- Update_Description: wording update -->