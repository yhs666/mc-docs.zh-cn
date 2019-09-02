---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 10/17/2018
ms.date: 03/04/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 7ffa8edc75e1bf0735641258d855e111de4b61f7
ms.sourcegitcommit: dcd11929ada5035d127be1ab85d93beb72909dc3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833215"
---
可以验证连接是否成功，方法是使用“Get-AzVirtualNetworkGatewayConnection”cmdlet，带或不带“-Debug”。 

1. 使用以下 cmdlet 示例，配置符合自己需要的值。 如果出现提示，请选择“A”运行“所有”。 在此示例中，“ -Name”是指要测试的连接的名称。

   ```azurepowershell
   Get-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
   ```
2. cmdlet 运行完毕后，查看该值。 在以下示例中，连接状态显示为“已连接”，且可以看到入口和出口字节数。
   
   ```
   "connectionStatus": "Connected",
   "ingressBytesTransferred": 33509044,
   "egressBytesTransferred": 4142431
   ```