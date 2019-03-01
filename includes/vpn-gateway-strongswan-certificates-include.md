---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 01/16/2019
ms.date: 03/04/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: bb1b5972310c05b0a16b712c54db1e5ff9912c0f
ms.sourcegitcommit: dcd11929ada5035d127be1ab85d93beb72909dc3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833225"
---
以下计算机配置用于执行下面的步骤：

  | | |
  |---|---|
  |Computer| Ubuntu Server 16.04<br>ID_LIKE=debian<br>PRETTY_NAME="Ubuntu 16.04.4 LTS"<br>VERSION_ID="16.04" |
  |依赖项| strongSwan |

#### <a name="1-install-strongswan"></a>1.安装 strongSwan

使用以下命令安装所需的 strongSwan 配置：

```
apt-get install strongswan-ikev2 strongswan-plugin-eap-tls
```

```
apt-get install libstrongswan-standard-plugins
```

```
apt-get install strongswan-pki
```

#### <a name="2-generate-keys-and-certificate"></a>2.生成密钥和证书

生成 CA 证书。

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```

打印 base64 格式的 CA 证书。 这是 Azure 支持的格式。 作为 P2S 配置的一部分，稍后需要将此证书上传到 Azure。

  ```
  openssl x509 -in caCert.pem -outform der | base64 -w0 ; echo
  ```

生成用户证书。

  ```
  export PASSWORD="password"
  export USERNAME="client"

  ipsec pki --gen --outform pem > "${USERNAME}Key.pem"
  ipsec pki --pub --in "${USERNAME}Key.pem" | ipsec pki --issue --cacert caCert.pem --cakey caKey.pem --dn "CN=${USERNAME}" --san "${USERNAME}" --flag clientAuth --outform pem > "${USERNAME}Cert.pem"
  ```

生成包含用户证书的 p12 捆绑包。 在后续步骤中使用客户端配置文件时将使用此捆绑包。

  ```
  openssl pkcs12 -in "${USERNAME}Cert.pem" -inkey "${USERNAME}Key.pem" -certfile caCert.pem -export -out "${USERNAME}.p12" -password "pass:${PASSWORD}"
  ```