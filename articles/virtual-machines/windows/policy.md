---
title: "在 Azure 中的 Windows VM 上通过策略强制执行安全措施 | Azure"
description: "如何向 Azure Resource Manager Windows 虚拟机应用策略"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0b71ba54-01db-43ad-9bca-8ab358ae141b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 06/28/2017
ms.date: 08/14/2017
ms.author: v-dazen
ms.openlocfilehash: 6b855555d0683a6c2b5e99e7176f2b34e446bf8e
ms.sourcegitcommit: f858adac6a7a32df67bcd5c43946bba5b8ec6afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2017
---
# <a name="apply-policies-to-windows-vms-with-azure-resource-manager"></a>使用 Azure 资源管理器向 Windows VM 应用策略
通过使用策略，组织可以在整个企业中强制实施各种约定和规则。 强制实施所需行为有助于消除风险，同时为组织的成功做出贡献。 本文介绍如何使用 Azure 资源管理器策略，为组织中的虚拟机定义所需的行为。

有关策略简介，请参阅[使用策略管理资源并控制访问](../../azure-resource-manager/resource-manager-policy.md)。

## <a name="define-policy-for-permitted-virtual-machines"></a>定义有关获准虚拟机的策略
若要确保组织的虚拟机与应用程序兼容，可以限制获准操作系统。 在以下策略示例中，只允许创建 Windows Server 2012 R2 数据中心虚拟机：

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/disks",
          "Microsoft.Compute/virtualMachines",
          "Microsoft.Compute/VirtualMachineScaleSets"
        ]
      },
      {
        "not": {
          "allOf": [
            {
              "field": "Microsoft.Compute/imagePublisher",
              "in": [
                "MicrosoftWindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "WindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "2012-Datacenter"
              ]
            },
            {
              "field": "Microsoft.Compute/imageVersion",
              "in": [
                "latest"
              ]
            }
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

使用通配符将上述策略修改为允许任何 Windows Server Datacenter 映像：

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

有关策略字段的信息，请参阅[策略别名](../../azure-resource-manager/resource-manager-policy.md#aliases)。

## <a name="define-policy-for-using-managed-disks"></a>定义有关使用托管磁盘的策略

如果需要使用托管磁盘，请使用以下策略：

```json
{
  "if": {
    "anyOf": [
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
            "exists": true
          }
        ]
      },
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/VirtualMachineScaleSets"
          },
          {
            "anyOf": [
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                "exists": true
              },
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                "exists": true
              }
            ]
          }
        ]
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a>后续步骤
* 定义策略规则后（如上述示例所示），需要创建策略定义并将其分配给作用域。 作用域可以是订阅、资源组或资源。
* 有关资源策略的简介，请参阅[资源策略概述](../../azure-resource-manager/resource-manager-policy.md)。
* 有关企业可如何使用 Resource Manager 有效管理订阅的指南，请参阅 [Azure 企业基架 - 出于合规目的监管订阅](../../azure-resource-manager/resource-manager-subscription-governance.md)。

<!--Update_Description: wording update-->