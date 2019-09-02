---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 01/16/2019
ms.date: 09/02/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 7efc68c9a552306536a54a63f650675fd989bf41
ms.sourcegitcommit: 3f0c63a02fa72fd5610d34b48a92e280c2cbd24a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70131684"
---
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