---
title: "监视 NSG 的操作、事件和计数器 | Azure"
description: "了解如何为 NSG 启用计数器、事件和操作日志记录"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 01/31/2017
ms.date: 03/31/2017
ms.author: v-dazen
ms.openlocfilehash: 730b557b295590b5b7c273f638a46359e38f67bc
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# <a name="log-analytics-for-network-security-groups-nsgs"></a>网络安全组 (NSG) 的日志分析

可以为 NSG 启用以下诊断日志类别：

* **事件：** 包含根据 MAC 地址向 VM 和实例角色应用 NSG 规则时所针对的条目。 每隔 60 秒收集一次这些规则的状态。
* **规则计数器：** 包含的条目适用于应用每个 NSG 规则以拒绝或允许流量的次数。

> [!NOTE]
> 诊断日志仅适用于通过 Azure Resource Manager 部署模型部署的 NSG。 对于通过经典部署模型部署的 NSG，不能启用诊断日志记录。 若要更好地了解这两种模型，可参考[了解 Azure 部署模型](../resource-manager-deployment-model.md)一文。

默认情况下，对通过任一 Azure 部署模型创建的 NSG 启用活动日志记录（以前称为审核日志或操作日志）。 若要在活动日志中确定完成了哪些 NSG 相关操作，请查看含有以下资源类型的条目： 

- Microsoft.ClassicNetwork/networkSecurityGroups 
- Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
- Microsoft.Network/networkSecurityGroups
- Microsoft.Network/networkSecurityGroups/securityRules 

阅读 [Azure 活动日志概述](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)一文可了解有关活动日志的详细信息。 

## <a name="enable-diagnostic-logging"></a>启用诊断日志记录

对于 *每个* 需要为其收集数据的 NSG，必须启用诊断日志记录。 如果还没有 NSG，请完成[创建网络安全组](virtual-networks-create-nsg-arm-pportal.md)一文中的步骤创建一个。 可以使用以下任意方法启用 NSG 诊断日志记录：

### <a name="azure-portal"></a>Azure 门户

若要使用门户启用日志记录，请登录到[门户](https://portal.azure.cn)。 单击“更多服务”，然后键入“网络安全组”。 选择要为其启用日志记录的 NSG。 选择 **NetworkSecurityGroupEvent** 和/或 **NetworkSecurityGroupRuleCounter** 日志类别。

### <a name="powershell"></a>PowerShell

评估以下信息，然后输入文中的一个命令：

- 可以根据需要替换以下 [text]，然后输入命令 `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]` 来确定要用于 `-ResourceId` 参数的值。 命令的 ID 输出看起来类似于 */subscriptions/[Subscription Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*。
- 如果只想收集日志类别的数据，请在本文中命令的末尾添加 `-Categories [category]`，其中 category 是 *NetworkSecurityGroupEvent* 或 *NetworkSecurityGroupRuleCounter*。 如果不使用 `-Categories` 参数，则可为两种日志类别启用数据收集。

### <a name="azure-command-line-interface-cli"></a>Azure 命令行接口 (CLI)

评估以下信息，然后输入文中的一个命令：

- 可以根据需要替换以下 [text]，然后输入命令 `azure network nsg show [resource-group-name] [nsg-name]` 来确定要用于 `-ResourceId` 参数的值。 命令的 ID 输出看起来类似于 */subscriptions/[Subscription Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*。
- 如果只想收集日志类别的数据，请在本文中命令的末尾添加 `-Categories [category]`，其中 category 是 *NetworkSecurityGroupEvent* 或 *NetworkSecurityGroupRuleCounter*。 如果不使用 `-Categories` 参数，则可为两种日志类别启用数据收集。

## <a name="logged-data"></a>记录的数据

将为两种日志写入 JSON 格式的数据。 针对每个日志类型写入的特定数据将列在以下各节：

### <a name="event-log"></a>事件日志
此日志包含的信息涉及哪些 NSG 规则根据 MAC 地址应用到 VM 和云服务角色实例。 以下示例数据是针对每个事件记录的：

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-B791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"3389-3389",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter-log"></a>规则计数器日志

此日志包含的信息涉及每个应用到资源的规则。 每次应用规则时，都会记录以下示例数据：

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]TESTRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```