---
title: 在 Azure 中查看 RBAC 更改的活动日志 | Microsoft Docs
description: 查看过去 90 天基于角色的访问控制更改的活动日志。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 04/23/2017
ms.date: 05/28/2018
ms.author: v-junlch
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7bf73d7ed378e6d7d6da1a06f772e6b4cb67b0e1
ms.sourcegitcommit: 6f42cd6478fde788b795b851033981a586a6db24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2018
ms.locfileid: "34559491"
---
# <a name="view-activity-logs-for-role-based-access-control-changes"></a>查看基于角色的访问控制更改的活动日志

只要有人在你的订阅中对角色定义或角色分配做出了更改，这些更改都会被记录在管理类别中的 [Azure 活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)中。 可以查看活动日志，了解基于角色的访问控制 (RBAC) 在过去 90 天发生的所有更改。

## <a name="operations-that-are-logged"></a>记录的操作

下面是记录在活动日志中的 RBAC 相关操作：

- 创建或更新自定义角色定义
- 删除自定义角色定义
- 创建角色分配
- 删除角色分配

## <a name="azure-portal"></a>Azure 门户

最简单的入手方式就是使用 Azure 门户查看活动日志。 以下屏幕截图显示了一个活动日志的示例，且已将日志筛选为显示“管理”类别以及角色定义和角色分配操作。 它还包括一个能将日志下载为 CSV 文件的链接。

![使用门户的活动日志 - 屏幕截图](./media/change-history-report/activity-log-portal.png)

有关详细信息，请参阅[在活动日志中查看事件](/azure-resource-manager/resource-group-audit)。

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

