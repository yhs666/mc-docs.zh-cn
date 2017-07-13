---
title: "修改本地网关 IP 地址前缀和 VPN 网关 IP 地址| Azure| CLI| Azure"
description: "本文介绍如何使用 Azure CLI 更改本地网络网关的 IP 地址前缀"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c7db48f-d09a-44e7-836f-1fb6930389df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 04/24/2017
ms.date: 05/31/2017
ms.author: v-dazen
ms.openlocfilehash: 7876c4f4b18c5aff9f407d9f72d52179f6591649
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 Azure CLI 修改本地网络网关设置
<a id="modify-local-network-gateway-settings-using-the-azure-cli" class="xliff"></a>

有时本地网络网关 AddressPrefix 或 GatewayIPAddress 的设置会变更。 本文演示如何修改本地网络网关设置。 也可在 Azure 门户或 PowerShell 中修改这些设置。

## 开始之前
<a id="before-you-begin" class="xliff"></a>

安装最新版本的 CLI 命令（2.0 或更高版本）。 有关安装 CLI 命令的信息，请参阅 [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)（安装 Azure CLI 2.0）。

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## 修改 IP 地址前缀
<a id="modify-ip-address-prefixes" class="xliff"></a>

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## 修改网关 IP 地址
<a id="modify-the-gateway-ip-address" class="xliff"></a>

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## 后续步骤
<a id="next-steps" class="xliff"></a>

可验证网关连接。 请参阅[验证网关连接](vpn-gateway-verify-connection-resource-manager.md)。