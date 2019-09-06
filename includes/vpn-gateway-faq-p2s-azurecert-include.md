---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 08/14/2019
ms.date: 09/02/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 27cabf58d4b77f37b550e8ecc59666c4eb221183
ms.sourcegitcommit: 3f0c63a02fa72fd5610d34b48a92e280c2cbd24a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70131717"
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-to-generate-certificates-for-point-to-site-connectivity"></a>是否可以使用自己的内部 PKI 根 CA 来生成用于点到站点连接的证书？

是的。 以前只可使用自签名根证书。 仍可上传 20 个根证书。

### <a name="can-i-use-certificates-from-azure-key-vault"></a>是否可以使用 Azure 密钥保管库中的证书？

否。

### <a name="what-tools-can-i-use-to-create-certificates"></a>可以使用哪些工具来创建证书？

可以使用企业 PKI 解决方案（内部 PKI）、Azure PowerShell、MakeCert 和 OpenSSL。

### <a name="certsettings"></a>是否有证书设置和参数的说明？

* **内部 PKI/企业 PKI 解决方案：** 请参阅[生成证书](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert)的步骤。

* **Azure PowerShell：** 请参阅 [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) 一文了解相关步骤。

* **MakeCert：** 请参阅 [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) 一文了解相关步骤。

* **OpenSSL：** 

    * 导出证书时，请务必将根证书转换为 Base64。

    * 对于客户端证书：

      * 创建私钥时，请将长度指定为 4096。
      * 创建证书时，对于 *-extensions* 参数，指定 *usr_cert*。