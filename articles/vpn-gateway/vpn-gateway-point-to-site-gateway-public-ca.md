---
title: 为 P2S 网关从自签名证书转换到公共 CA 证书 | Azure VPN 网关 |Microsoft Docs
description: 本文可帮助你为 P2S 网关成功转换到新的公共 CA 证书。
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: conceptual
origin.date: 02/07/2019
ms.date: 02/18/2019
ms.author: v-jay
ms.openlocfilehash: c39b9566ad06744fb0806999ccfa351da6e7b911
ms.sourcegitcommit: 2bcf3b51503f38df647c08ba68589850d91fedfe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2019
ms.locfileid: "56303096"
---
# <a name="transition-from-self-signed-to-public-ca-certificates-for-p2s-gateways"></a>为 P2S 网关从自签名证书转换到公共 CA 证书

Azure VPN 网关不再向用于 P2S 连接的网关颁发自签名证书。 现在，颁发的证书由公共证书颁发机构 (CA) 签名。 但是，较旧的网关可能仍在使用自签名证书。 这些自签名证书已接近其到期日期，并且必须转换到公共 CA 证书。

以前，网关的自签名证书需要每 18 个月更新一次。 然后，VPN 客户端配置文件需要生成并重新部署到所有 P2S 客户端。 转换到公共 CA 证书将消除此限制。 除了证书的转换之外，此更改还可改进平台、优化指标，并且提高稳定性。

只有较旧的网关会受此更改影响。 如果网关证书需要转换，你将在 Azure 门户中收到相关信息或 toast。 可以通过执行本文中的步骤查看网关是否受影响。

>[!IMPORTANT]
>该转换已计划于 2019 年 3 月 9 日 18:00 UTC 开始。 如果需要不同的时间窗口，可以创建一个支持案例。 可以请求以下窗口之一：
>
>* 2 月 25 日 06:00 UTC
>* 2 月 25 日 18:00 UTC
>* 3 月 1 日 06:00 UTC
>* 3 月 1 日 18:00 UTC
>
>**所有剩余网关将于 2019 年 3 月 9 日 18:00 UTC 开始转换**。
>

## <a name="1-verify-your-certificate"></a>1.验证证书

1. 查看你是否受此更新影响。 按照[这篇文章](point-to-site-vpn-client-configuration-azure-cert.md)中的步骤下载当前 VPN 客户端配置。

2. 打开或解压缩 zip 文件，然后浏览到“Generic”文件夹。 在 Generic 文件夹中，你将看到两个文件，其中之一是 *VPNSettings.xml*。
3. 在任意 xml 查看器/编辑器中打开 *VPNSettings.xml*。 在该 xml 文件中，搜索以下字段：

  * `<ServerCertRootCn>DigiCert Global Root CA</ServerCertRootCn>`
  * `<ServerCertIssuerCn>DigiCert Global Root CA</ServerCertIssuerCn>`
4. 如果 *ServerCertRotCn* 和 *ServerCertIssuerCn* 是“DigiCert Global Root CA”，那么你不受此更新影响，无需继续执行本文中的步骤。 但是，如果这两个字段显示其他内容，那么你的网关证书属于此更新的一部分，将进行转换。

## <a name="2-check-certificate-transition-schedule"></a>2.检查证书转换计划

如果证书属于此更新的一部分，那么网关证书将进行转换。 请参阅有关转换时间的**重要**说明。 更新后，P2S 客户端将无法使用其旧配置文件进行连接。 必须生成新的 VPN 客户端配置文件，并将其安装在客户端上。

## <a name="3-generate-vpn-client-configuration-profile"></a>3.生成 VPN 客户端配置文件

已转换该证书后，请从 Azure 门户下载新的 VPN 配置文件（VPN 客户端配置文件）。 有关步骤，请参阅[创建和安装 VPN 客户端配置文件](point-to-site-vpn-client-configuration-azure-cert.md)。 不需要生成新的客户端证书。

## <a name="4-deploy-vpn-client-profile"></a>4.部署 VPN 客户端配置文件

将新的配置文件部署到所有的点到站点 VPN 客户端。 VPN 客户端将无法连接，直到点到站点连接的新 VPN 配置文件已下载并部署到客户端设备为止。 VPN 客户端计算机上已安装的客户端证书不需要重新颁发。

## <a name="5-connect-the-vpn-client"></a>5.连接 VPN 客户端

像通常那样从 VPN 客户端连接到 Azure。