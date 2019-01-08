---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 11/30/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 9d7637d3be9ecc3dc43057bd2ecc6670cacde4da
ms.sourcegitcommit: 0a5a7daaf864ef787197f2b8e62539786b6835b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/20/2018
ms.locfileid: "53711670"
---
1. 在 [Azure 门户](http://portal.azure.cn)中，导航到要为其创建虚拟网关的资源管理器虚拟网络。

2. 在虚拟网络页的“设置”部分中，选择“子网”以展开“子网”页。

3. 在“子网”页上，选择“网关子网”以打开“添加子网”页。

   ![添加网关子网](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsub.png "添加网关子网")

4. 子网的**名称**自动填充为值 *GatewaySubnet*。 Azure 需要此值才能识别作为网关子网的子网。 根据配置要求调整自动填充的“地址范围”值，然后选择“确定”以创建该子网。

   ![添加子网](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addsubnetgw.png "添加子网")