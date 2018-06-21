---
title: 保护物联网部署 | Azure
description: 本文详细说明如何保护 IoT 部署
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 95c23341-16b0-4954-b3f2-d2e82ab7b367
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 01/17/2018
ms.author: v-yiso
ms.date: 03/12/2018
ms.openlocfilehash: 66c5a8c9682afa131e8e9e37366f12890f540ad4
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
ms.locfileid: "29731222"
---
[!INCLUDE [iot-secure-your-deployment](../../includes/iot-secure-your-deployment.md)]

## <a name="iot-suite-cipher-suites"></a>IoT 套件密码套件

Azure IoT 套件支持以下密码套件（按此顺序）。

| 密码套件 | Length |
| --- | --- |
| TLS\_ECDHE\_RSA\_ 与 \_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1（等于 7680 位 RSA）FS |256 |
| TLS\_ECDHE\_RSA\_ 与 \_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1（等于 3072 位 RSA) FS |128 |
| TLS\_ECDHE\_RSA\_ 与 \_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1（等于 7680 位 RSA）FS |256 |
| TLS\_ECDHE\_RSA\_ 与 \_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1（等于 3072 位 RSA）FS |128 |
| TLS\_RSA\_ 与 \_AES\_256\_GCM\_SHA384 (0x9d) |256 |
| TLS\_RSA\_ 与 \_AES\_128\_GCM\_SHA256 (0x9c) |128 |
| TLS\_RSA\_ 与 \_AES\_256\_CBC\_SHA256 (0x3d) |256 |
| TLS\_RSA\_ 与 \_AES\_128\_CBC\_SHA256 (0x3c) |128 |
| TLS\_RSA\_ 与 \_AES\_256\_CBC\_SHA (0x35) |256 |
| TLS\_RSA\_ 与 \_AES\_128\_CBC\_SHA (0x2f) |128 |
| TLS\_RSA\_ 与 \_3DES\_EDE\_CBC\_SHA (0xa) |112 |

## <a name="see-also"></a>另请参阅
还可以浏览 IoT 套件预配置的解决方案的一些其他特性和功能：

- [预见性维护预配置解决方案概述][lnk-predictive-overview]
- [有关 IoT 套件的常见问题][lnk-faq]

可以在 IoT 中心开发人员指南的 [控制 IoT 中心的访问权限][lnk-devguide-security] 中阅读 IoT 中心安全性的相关信息。

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md