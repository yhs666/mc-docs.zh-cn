---
title: 获取资源更改
description: 了解如何查找资源更改时间、获取已更改属性的列表以及评估差异。
services: resource-graph
author: DCtheGeek
ms.author: v-yiso
origin.date: 10/09/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: af88c88cd8cf98b1bb74f15a3e10beb53e7ff4d7
ms.sourcegitcommit: cf73284534772acbe7a0b985a86a0202bfcc109e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74884999"
---
# <a name="get-resource-changes"></a>获取资源更改

在日常使用、重新配置甚至是重新部署的过程中，资源都会发生更改。
更改可能是个人或者自动化过程做出的。 大部分更改是依设计做出的，但有时并非如此。 Azure Resource Graph 提供过去 14 天的更改历史记录，可让你：

- 查找在 Azure 资源管理器属性中检测到更改的时间
- 对于每个资源更改，请参阅属性更改详细信息
- 查看检测到的更改之前和之后的资源的完整比较

对于以下示例场景，更改检测和详细信息很有作用：

- 在事件管理期间了解潜在相关的更改。  查询特定时段内的更改事件，以及评估更改详细信息。
- 使配置管理数据库（简称为 CMDB）保持最新。 无需按计划的频率刷新所有资源及其完整的属性集，只获取更改的内容。
- 了解当某个资源更改了合规状态时可能已更改的其他属性。 评估这些附加属性可以洞察可能需要通过 Azure Policy 定义进行管理的其他属性。

本文介绍如何通过 Resource Graph 的 SDK 收集此信息。 若要在 Azure 门户中查看此信息，请参阅 Azure Policy 的[更改历史记录](../../policy/how-to/determine-non-compliance.md#change-history-preview)或 Azure 活动日志[更改历史记录](../../../azure-monitor/platform/activity-log-view.md#azure-portal)。


> [!NOTE]
> Resource Graph 中的更改详细信息适用于资源管理器属性。 若要跟踪虚拟机内部的更改，请参阅 Azure Policy 的 [VM 的来宾配置](../../policy/concepts/guest-configuration.md)。

> [!IMPORTANT]
> Azure Resource Graph 中的更改历史记录目前以公共预览版提供。

## <a name="find-detected-change-events-and-view-change-details"></a>查找检测到的更改事件并查看更改详细信息

查看资源发生的更改的第一步是查找某个时段内与该资源相关的更改事件。 每个更改事件还包含有关资源的哪些信息发生更改的详细信息。 此步骤是通过 **resourceChanges** REST 终结点完成的。

**resourceChanges** 终结点接受请求正文中的以下参数：

- **resourceId** \[必需\]：要查看其更改的 Azure 资源。
- **interval** \[必需\]：包含检查更改事件的开始日期和结束日期（采用**祖鲁时区 (Z)** ）的属性。  
- **fetchPropertyChanges**（可选）：一个布尔值属性，它设置响应对象是否包括属性更改。

请求正文示例：

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "interval": {
        "start": "2019-09-28T00:00:00.000Z",
        "end": "2019-09-29T00:00:00.000Z"
    },
    "fetchPropertyChanges": true
}
```

对于上述请求正文，**resourceChanges** 的 REST API URI 是：

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChanges?api-version=2018-09-01-preview
```

响应如以下示例所示：

```json
{
    "changes": [
        {
            "changeId": "{\"beforeId\":\"3262e382-9f73-4866-a2e9-9d9dbee6a796\",\"beforeTime\":\"2019-09-28T00:45:35.012Z\",\"afterId\":\"6178968e-981e-4dac-ac37-340ee73eb577\",\"afterTime\":\"2019-09-28T00:52:53.371Z\"}",
            "beforeSnapshot": {
                "snapshotId": "3262e382-9f73-4866-a2e9-9d9dbee6a796",
                "timestamp": "2019-09-28T00:45:35.012Z"
            },
            "afterSnapshot": {
                "snapshotId": "6178968e-981e-4dac-ac37-340ee73eb577",
                "timestamp": "2019-09-28T00:52:53.371Z"
            },
            "changeType": "Create"
        },
        {
            "changeId": "{\"beforeId\":\"a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c\",\"beforeTime\":\"2019-09-28T00:43:38.366Z\",\"afterId\":\"3262e382-9f73-4866-a2e9-9d9dbee6a796\",\"afterTime\":\"2019-09-28T00:45:35.012Z\"}",
            "beforeSnapshot": {
                "snapshotId": "a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c",
                "timestamp": "2019-09-28T00:43:38.366Z"
            },
            "afterSnapshot": {
                "snapshotId": "3262e382-9f73-4866-a2e9-9d9dbee6a796",
                "timestamp": "2019-09-28T00:45:35.012Z"
            },
            "changeType": "Delete"
        },
        {
            "changeId": "{\"beforeId\":\"b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c\",\"beforeTime\":\"2019-09-28T00:43:15.518Z\",\"afterId\":\"a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c\",\"afterTime\":\"2019-09-28T00:43:38.366Z\"}",
            "beforeSnapshot": {
                "snapshotId": "b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c",
                "timestamp": "2019-09-28T00:43:15.518Z"
            },
            "afterSnapshot": {
                "snapshotId": "a00f5dac-86a1-4d86-a1c5-a9f7c8147b7c",
                "timestamp": "2019-09-28T00:43:38.366Z"
            },
            "propertyChanges": [
                {
                    "propertyName": "tags.org",
                    "afterValue": "compute",
                    "changeCategory": "User",
                    "changeType": "Insert"
                },
                {
                    "propertyName": "tags.team",
                    "afterValue": "ARG",
                    "changeCategory": "User",
                    "changeType": "Insert"
                }
            ],
            "changeType": "Update"
        },
        {
            "changeId": "{\"beforeId\":\"19d12ab1-6ac6-4cd7-a2fe-d453a8e5b268\",\"beforeTime\":\"2019-09-28T00:42:46.839Z\",\"afterId\":\"b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c\",\"afterTime\":\"2019-09-28T00:43:15.518Z\"}",
            "beforeSnapshot": {
                "snapshotId": "19d12ab1-6ac6-4cd7-a2fe-d453a8e5b268",
                "timestamp": "2019-09-28T00:42:46.839Z"
            },
            "afterSnapshot": {
                "snapshotId": "b37a90d1-7ebf-41cd-8766-eb95e7ee4f1c",
                "timestamp": "2019-09-28T00:43:15.518Z"
            },
            "propertyChanges": [{
                "propertyName": "tags.cgtest",
                "afterValue": "hello",
                "changeCategory": "User",
                "changeType": "Insert"
            }],
            "changeType": "Update"
        }
    ]
}
```

**resourceId** 的每个检测到的更改事件都具有以下属性：

- **changeId** - 此值对于该资源是唯一的。 **changeId** 字符串有时可能包含其他属性，只能保证它是唯一的。
- **beforeSnapshot** - 包含检测到更改之前创建的资源快照的 **snapshotId** 和 **timestamp**。
- **afterSnapshot** - 包含检测到更改之后创建的资源快照的 **snapshotId** 和 **timestamp**。
- **changeType** - 描述已针对 **beforeSnapshot** 和 **afterSnapshot** 之间的整个更改记录检测到的更改的类型。 值为：_Create_、_Update_ 和 _Delete_。 仅当 **changeType** 为 _Update_ 时，才包括 **propertyChanges** 属性数组。
- **propertyChanges** - 此属性数组提供了 **beforeSnapshot** 和 **afterSnapshot** 之间已更新的所有资源属性的详细信息：
  - **propertyName** - 已更改的资源属性的名称。
  - **changeCategory** - 描述哪类更改者执行了更改。 值为：_System_ 和 _User_。
  - **changeType** - 描述已为单个资源属性检测到的更改的类型。
    值为：_Insert_、_Update_、_Remove_。
  - **beforeValue** - **beforeSnapshot** 中资源属性的值。 当 **changeType** 为 _Insert_ 时不显示。
  - **afterValue** - **afterSnapshot** 中的资源属性的值。 当 **changeType** 为 _Remove_ 时不显示。

## <a name="compare-resource-changes"></a>比较资源更改

有了来自 **resourceChanges** 终结点的 **changeId** 后，可以使用 **resourceChangeDetails** REST 终结点获取更改资源之前和之后的资源快照。

**resourceChangeDetails** 终结点要求在请求正文中使用两个参数：

- **resourceId**：要比较其更改的 Azure 资源。
- **changeId**：从 **resourceChanges** 中收集的 **resourceId** 的唯一更改事件。

请求正文示例：

```json
{
    "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"afterTime\":'2019-05-10T00:00:00.000Z\"}"
}
```

对于上述请求正文，**resourceChangeDetails** 的 REST API URI 是：

```http
POST https://management.azure.com/providers/Microsoft.ResourceGraph/resourceChangeDetails?api-version=2018-09-01-preview
```

响应如以下示例所示：

```json
{
    "changeId": "{\"beforeId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-09T00:00:00.000Z\",\"afterId\":\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\",\"beforeTime\":'2019-05-10T00:00:00.000Z\"}",
    "beforeSnapshot": {
        "timestamp": "2019-03-29T01:32:05.993Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "chinaeast",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": false,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "chinaeast",
                "statusOfPrimary": "available"
            }
        }
    },
    "afterSnapshot": {
        "timestamp": "2019-03-29T01:54:24.42Z",
        "content": {
            "sku": {
                "name": "Standard_LRS",
                "tier": "Standard"
            },
            "kind": "Storage",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
            "name": "mystorageaccount",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "chinaeast",
            "tags": {},
            "properties": {
                "networkAcls": {
                    "bypass": "AzureServices",
                    "virtualNetworkRules": [],
                    "ipRules": [],
                    "defaultAction": "Allow"
                },
                "supportsHttpsTrafficOnly": true,
                "encryption": {
                    "services": {
                        "file": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        },
                        "blob": {
                            "enabled": true,
                            "lastEnabledTime": "2018-07-27T18:37:21.8333895Z"
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "provisioningState": "Succeeded",
                "creationTime": "2018-07-27T18:37:21.7708872Z",
                "primaryEndpoints": {
                    "blob": "https://mystorageaccount.blob.core.windows.net/",
                    "queue": "https://mystorageaccount.queue.core.windows.net/",
                    "table": "https://mystorageaccount.table.core.windows.net/",
                    "file": "https://mystorageaccount.file.core.windows.net/"
                },
                "primaryLocation": "chinaeast",
                "statusOfPrimary": "available"
            }
        }
    }
}
```

**beforeSnapshot** 和 **afterSnapshot** 分别提供快照创建时间以及当时的属性。 更改是在这些快照之间的某个时间点发生的。 在以上示例中我们可以看到，更改的属性是 **supportsHttpsTrafficOnly**。

若要对结果进行比较，请使用 **resourceChanges** 中的 **changes** 属性，或评估 **resourceChangeDetails** 中每个快照的 **content** 部分，以确定差异。 如果对快照进行比较，**timestamp** 始终会显示为差异，不过这符合预期。

## <a name="next-steps"></a>后续步骤

- 在[初学者查询](../samples/starter.md)中了解使用的语言。
- 在[高级查询](../samples/advanced.md)中了解高级用法。
- 详细了解如何[浏览资源](../concepts/explore-resources.md)。