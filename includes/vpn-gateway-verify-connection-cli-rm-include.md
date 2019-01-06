---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 3417cc0ce1009435aee22aaeba36091facf70493
ms.sourcegitcommit: 0a5a7daaf864ef787197f2b8e62539786b6835b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/20/2018
ms.locfileid: "53711681"
---
可使用 [az network vpn-connection show](/cli/network/vpn-connection#show) 命令来验证连接是否成功。 在此示例中，“--name”是指要测试的连接的名称。 当连接处于建立过程中时，连接状态会显示“正在连接”。 建立连接后，状态将更改为“已连接”。

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```
