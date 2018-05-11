---
title: 适用于虚拟网络的 Azure CLI 示例 | Azure
description: 适用于虚拟网络的 Azure CLI 示例。
services: virtual-network
documentationcenter: virtual-network
author: rockboyfor
manager: digimobile
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
origin.date: 03/20/2018
ms.date: 05/07/2018
ms.author: v-yeche
ms.openlocfilehash: 5d085383f4d3bb57d82554c8772dab3c11f5bdb7
ms.sourcegitcommit: 0b63440e7722942ee1cdabf5245ca78759012500
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2018
---
# <a name="azure-cli-samples-for-virtual-network"></a>适用于虚拟网络的 Azure CLI 示例

下表包含指向使用 Azure CLI 命令的 bash 脚本的链接：

| | |
|----|----|
| [为多层应用程序创建虚拟网络](./scripts/virtual-network-cli-sample-multi-tier-application.md) | 创建包含前端和后端子网的虚拟网络。 传入前端子网的流量仅限 HTTP 和 SSH，而传入后端子网的流量限于 MySQL、端口 3306。 |
| [两个对等虚拟网络](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | 在同一区域中创建并连接两个虚拟网络。 |
| [通过网络虚拟设备的路由流量](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | 创建包含前端和后端子网的虚拟网络，以及可在这两个子网之间路由流量的 VM。 |
| [筛选入站和出站 VM 网络流量](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | 创建包含前端和后端子网的虚拟网络。 前端子网的入站网络流量仅限于 HTTP、HTTPS 和 SSH。 不允许从后端子网到 Internet 的出站流量。 |
<!-- Update_Description: new articles on cli samples  -->
<!--ms.date: 05/07/2018-->