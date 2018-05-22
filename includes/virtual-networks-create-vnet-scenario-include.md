---
title: include 文件
description: include 文件
services: virtual-network
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 04/13/2018
ms.date: 05/07/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: e4d87f784e5201b5dc49b79d05453932149bf39a
ms.sourcegitcommit: 6f08b9a457d8e23cf3141b7b80423df6347b6a88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2018
---
## <a name="scenario"></a>方案

为了说明如何创建 VNet 和子网，本文档使用以下方案：

![VNet 方案](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

在此方案中，将创建名为 **TestVNet** 的 VNet，其中有保留 CIDR 块 **192.168.0.0./16**。 该 VNet 包含以下子网： 

* FrontEnd，使用 192.168.1.0/24 作为其 CIDR 块。
* BackEnd，使用 192.168.2.0/24 作为其 CIDR 块。