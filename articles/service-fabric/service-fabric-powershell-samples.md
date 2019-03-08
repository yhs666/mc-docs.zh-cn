---
title: Azure PowerShell 示例 - Service Fabric | Azure
description: Azure PowerShell 示例 - Service Fabric
services: service-fabric
documentationcenter: service-fabric
author: rockboyfor
manager: digimobile
editor: ''
tags: ''
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: service-fabric
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: service-fabric
origin.date: 11/29/2018
ms.date: 03/04/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 382aac87ab49e727f8ffae51c7aa8cc32b5b9606
ms.sourcegitcommit: f1ecc209500946d4f185ed0d748615d14d4152a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463464"
---
# <a name="azure-powershell-samples"></a>Azure PowerShell 示例

下表包含用于创建和管理 Service Fabric 群集、应用程序和服务的 PowerShell 脚本示例的链接。

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| 创建群集 ||
| [创建群集 (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| 创建 Azure Service Fabric 群集。 |
| **管理群集、节点和基础结构** ||
| [添加应用程序证书](./scripts/service-fabric-powershell-add-application-certificate.md)| 将应用程序 X.509 证书添加到群集中的所有节点。 |
| [更新群集 VM 上的 RDP 端口范围](./scripts/service-fabric-powershell-change-rdp-port-range.md)|更改已部署群集中群集节点 VM 上的 RDP 端口范围。|
| [更新群集节点 VM 的管理员用户和密码](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | 更新群集节点 VM 的管理员用户名和密码。 |
| [打开负载均衡器的端口](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | 在 Azure 负载均衡器中打开应用程序端口，以允许特定端口上的入站流量。 |
| [创建入站网络安全组规则](./scripts/service-fabric-powershell-add-nsg-rule.md) | 创建入站网络安全组规则，以允许入站流量发往特定端口上的群集。 |
| **管理应用程序** ||
| [部署应用程序](./scripts/service-fabric-powershell-deploy-application.md)| 将应用程序部署到群集。|
| [升级应用程序](./scripts/service-fabric-powershell-upgrade-application.md)| 升级应用程序。|
| [删除应用程序](./scripts/service-fabric-powershell-remove-application.md)| 从群集中删除应用程序。|

<!--Update_Description: update meta properties -->
