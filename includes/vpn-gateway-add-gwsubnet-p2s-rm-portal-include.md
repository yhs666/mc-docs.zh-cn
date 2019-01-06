---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 10/21/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: e885362be996112371e4f0d3c595629dff4286ab
ms.sourcegitcommit: 0a5a7daaf864ef787197f2b8e62539786b6835b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/20/2018
ms.locfileid: "53711700"
---
1. 在[门户](http://portal.azure.cn)中，导航到要为其创建虚拟网关的 Resource Manager 虚拟网络。
2. 在 VNet 页的“设置”部分单击“子网”，展开“子网”页。
3. 在“子网”页中，单击“+网关子网”打开“添加子网”页。 

   ![添加网关子网](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "添加网关子网")
4. 子网的“名称”自动填充为值“GatewaySubnet”。 Azure 需要此值才能识别作为网关子网的子网。 调整自动填充的**地址范围**值，使其匹配配置要求。 请不要配置路由表或服务终结点。

   ![添加子网](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "添加子网")
5. 单击页面底部的“确定”以创建子网。
