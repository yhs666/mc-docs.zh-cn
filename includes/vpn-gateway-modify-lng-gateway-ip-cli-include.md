---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 03/04/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 272e19b474695cb6554ff209fba225d7d5d294ff
ms.sourcegitcommit: dcd11929ada5035d127be1ab85d93beb72909dc3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833222"
---
### <a name="to-modify-the-local-network-gateway-gatewayipaddress"></a>修改本地网关的“gatewayIpAddress”

如果要连接的 VPN 设备已更改其公共 IP 地址，则需根据该更改修改本地网关。 可以更改网关 IP 地址而不删除现有的 VPN 网关连接（如果有）。 要修改网关 IP 地址，请使用 [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway) 命令将值“Site2”和“TestRG1”替换为自己的值。

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

验证输出中的 IP 地址是否正确：

```
"gatewayIpAddress": "23.99.222.170",
```
