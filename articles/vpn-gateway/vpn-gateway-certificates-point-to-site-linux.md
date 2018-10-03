---
title: 为点到站点连接生成和导出证书：Linux：CLI：Azure | Microsoft Docs
description: 使用 Linux (strongSwan) CLI 创建自签名根证书、导出公钥以及生成客户端证书。
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: article
origin.date: 09/06/2018
ms.date: 10/01/2018
ms.author: v-jay
ms.openlocfilehash: b98999e62e6cd0f708a27e2c260d3845674814c4
ms.sourcegitcommit: 04071a6ddf4e969464d815214d6fdd9813c5c5a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2018
ms.locfileid: "47426491"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-linux-strongswan-cli"></a>使用 Linux strongSwan CLI 为点到站点连接生成和导出证书

点到站点连接使用证书进行身份验证。 本文介绍了如何使用 Linux CLI 和 strongSwan 创建自签名根证书并生成客户端证书。 如果要查找不同的证书说明，请参阅 [Powershell](vpn-gateway-certificates-point-to-site.md) 或 [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) 文章。

> [!NOTE]
> 本文中的步骤需要 strongSwan。
>

用于本文中步骤的计算机配置如下所述：

| | |
|---|---|
|**计算机**| Ubuntu Server 16.04<br>ID_LIKE=debian<br>PRETTY_NAME="Ubuntu 16.04.4 LTS"<br>VERSION_ID="16.04" |
|**依赖项**| apt-get install strongswan-ikev2 strongswan-plugin-eap-tls<br>apt-get install libstrongswan-standard-plugins |

## <a name="install-strongswan"></a>安装 strongSwan

1. `apt-get install strongswan-ikev2 strongswan-plugin-eap-tls`
2. `apt-get install libstrongswan-standard-plugins`

有关如何使用 GUI 安装 strongSwan 的信息，请参阅[客户端配置](point-to-site-vpn-client-configuration-azure-cert.md#install)一文中的步骤。

## <a name="generate-keys-and-certificate"></a>生成密钥和证书

1. 生成 CA 证书。

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```
2. 打印 base64 格式的 CA 证书。 这是 Azure 支持的格式。 作为 P2S 配置的一部分，稍后需要将此证书上传到 Azure。

  ```
  openssl x509 -in caCert.pem -outform der | base64 -w0 ; echo
  ```
3. 生成用户证书。

  ```
  export PASSWORD="password"
  export USERNAME="client"

  ipsec pki --gen --outform pem > "${USERNAME}Key.pem"
  ipsec pki --pub --in "${USERNAME}Key.pem" | ipsec pki --issue --cacert caCert.pem --cakey caKey.pem --dn "CN=${USERNAME}" --san "${USERNAME}" --flag clientAuth --outform pem > "${USERNAME}Cert.pem"
  ```
4. 生成包含用户证书的 p12 捆绑包。 在后续步骤中处理[客户端配置文件](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli)时将使用此捆绑包。

  ```
  openssl pkcs12 -in "${USERNAME}Cert.pem" -inkey "${USERNAME}Key.pem" -certfile caCert.pem -export -out "${USERNAME}.p12" -password "pass:${PASSWORD}"
  ```

## <a name="next-steps"></a>后续步骤

继续执行点到站点配置以[创建和安装 VPN 客户端配置文件](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli)。