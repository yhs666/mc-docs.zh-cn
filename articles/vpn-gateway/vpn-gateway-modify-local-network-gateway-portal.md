---
title: 修改本地网络网关 IP 地址前缀和 VPN 网关 IP 地址 | Azure| 门户 | Microsoft Docs
description: 本文介绍了如何使用 Azure 门户更改本地网络网关的 IP 地址前缀。
services: vpn-gateway
documentationcenter: na
author: WenJason
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 06/19/2017
ms.date: 12/10/2018
ms.author: v-jay
ms.openlocfilehash: e7c3379c476707d6c94db18e8d0f99d1590e0a91
ms.sourcegitcommit: 5f2849d5751cb634f1cdc04d581c32296e33ef1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "53028960"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>使用 Azure 门户修改本地网络网关设置

有时本地网络网关 AddressPrefix 或 GatewayIPAddress 的设置会变更。 本文演示如何修改本地网络网关设置。 还可以通过选择以下列表中的其他选项，使用另一种方法来修改这些设置：

在删除连接之前，可能需要下载用于连接设备的配置，以便获取已定义的 PSK。 这样，便无需在另一端重新定义它。

> [!div class="op_single_selector"]
> * [Azure 门户](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="ipaddprefix"></a>修改 IP 地址前缀

修改 IP 地址前缀时，执行的步骤取决于本地网络网关是否具有连接。

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="gwip"></a>修改网关 IP 地址

如果要连接的 VPN 设备已更改其公共 IP 地址，则需根据该更改修改本地网关。 更改公共 IP 地址时，执行的步骤取决于本地网络网关是否具有连接。

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>后续步骤

可验证网关连接。 请参阅[验证网关连接](vpn-gateway-verify-connection-resource-manager.md)。