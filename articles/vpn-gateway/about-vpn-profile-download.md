---
title: Azure VPN 网关：关于 P2S VPN 客户端配置文件
description: 这有助于你使用客户端配置文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: article
origin.date: 11/04/2019
ms.date: 12/02/2019
ms.author: v-jay
ms.openlocfilehash: 596a2ceec5c632701aedcc9ec1859b811cbe7a93
ms.sourcegitcommit: fac243483f641e1d01646a30197522a60599d837
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2019
ms.locfileid: "74553073"
---
# <a name="about-p2s-vpn-client-profiles"></a>关于 P2S VPN 客户端配置文件

下载的配置文件包含配置 VPN 连接所需的信息。 本文将有助于你获取和了解 VPN 客户端配置文件所需的信息。

## <a name="1-download-the-file"></a>1.下载文件

运行以下命令。 将结果 URL 复制到浏览器，以便下载 zip 配置文件。

```azurepowershell
$profile = New-AzVpnClientConfiguration -ResourceGroupName AADAuth -Name AADauthGW -AuthenticationMethod "EapTls"
   
$PROFILE.VpnProfileSASUrl
```

## <a name="2-extract-the-zip-file"></a>2.解压缩 zip 文件
## <a name="folder-contents"></a>文件夹内容

* **Generic 文件夹**包含公共服务器证书和 VpnSettings.xml 文件。 VpnSettings.xml 文件包含配置通用客户端所需的信息。

* 下载的 zip 文件可能还包含 **WindowsAmd64** 和 **WindowsX86** 文件夹。 这些文件夹包含 Windows 客户端的 SSTP 和 IKEv2 的安装程序。 需要有客户端的管理员权限才能安装它们。

## <a name="next-steps"></a>后续步骤

有关点到站点的详细信息，请参阅[关于点到站点](point-to-site-about.md)。
