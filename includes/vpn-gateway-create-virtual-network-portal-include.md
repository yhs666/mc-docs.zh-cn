---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 08/02/2019
ms.date: 12/02/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: eeaa419cbb584bfedc09f732dea01fe25df8ceed
ms.sourcegitcommit: 9597d4da8af58009f9cef148a027ccb7b32ed8cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2019
ms.locfileid: "74655485"
---
若要使用 Azure 门户在 Resource Manager 部署模型中创建 VNet，请执行以下步骤。 如果是在教程中使用这些步骤，请使用**示例值**。 如果并非在教程中使用这些步骤，请务必将其中的值替换为自己的值。 有关使用虚拟网络的详细信息，请参阅 [虚拟网络概述](../articles/virtual-network/virtual-networks-overview.md)。

>[!NOTE]
>为了让此 VNet 连接到本地位置，需与本地网络管理员协调操作，指定一个 IP 地址范围，将其专用于此虚拟网络。 如果 VPN 连接的两侧存在重复的地址范围，则流量不会按预期的方式路由。 另外，若要将此 VNet 连接到其他 VNet，则地址空间不能与其他 VNet 重叠。 请注意对网络配置进行相应的计划。
>

1. 在 [Azure 门户](https://portal.azure.com)菜单中，选择“创建资源”  。 

   ![在 Azure 门户中创建资源](./media/vpn-gateway-create-virtual-network-portal-include/azure-portal-create-resource.png)
2. 在“在市场中搜索”  字段中，键入“虚拟网络”。 从返回的列表中找到“虚拟网络”  ，单击打开“虚拟网络”  页。
3. 单击**创建**。 这会打开“创建虚拟网络”页  。
4. 在“创建虚拟网络”  页上，配置 VNet 设置。 填写字段时，如果在字段中输入的字符有效，红色感叹号标记会变成绿色对钩标记。 使用以下值：

   - **名称**：VNet1
   - **地址空间**：10.1.0.0/16
   - **订阅**：确认列出的订阅是你想要使用的订阅。 可以使用下拉列表更改订阅。
   - **资源组**：TestRG1（单击“新建”  以创建新组）
   - **位置**：中国北部
   - **子网**：前端
   - **地址范围**：10.1.0.0/24

   ![创建虚拟网络页](./media/vpn-gateway-create-virtual-network-portal-include/create-virtual-network1.png)
5. 将 DDoS 保留为“基本”，将服务终结点保留为“禁用”，并将防火墙保留为“禁用”。
6. 单击“创建”以创建该 VNet  。