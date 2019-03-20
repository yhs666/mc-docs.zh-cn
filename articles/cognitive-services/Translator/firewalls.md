---
title: 在防火墙后面翻译 - 文本翻译 API
titlesuffix: Azure Cognitive Services
description: 使用文本翻译 API 在防火墙后面翻译。
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
origin.date: 02/21/2019
ms.date: 03/12/2019
ms.author: v-junlch
ms.openlocfilehash: 40e6e9595b3692e38cbdc8a3b1a3c145f8ec2140
ms.sourcegitcommit: 58b6ce9ef2aae78a729f1c59f30e244e2458f90d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2019
ms.locfileid: "57964998"
---
# <a name="how-to-translate-behind-ip-firewalls-with-the-translator-text-api"></a>如何使用文本翻译 API 在防火墙后面翻译

文本翻译 API 可以使用域名或 IP 筛选在防火墙后面翻译。 域名筛选是首选方法。 我们建议不要在经过 IP 筛选的防火墙后面运行 Microsoft Translator。 安装程序在将来可能会发生中断，恕不另行通知。

## <a name="translator-ip-addresses"></a>Translator IP 地址
自 2018 年 11 月 20 日起，api.translator.azure.cn - Microsoft 文本翻译 API 的 IP 地址：

* **亚太区：** 40.90.139.163, 104.44.89.44
* **欧洲：** 40.90.138.4, 40.90.141.99
* **北美：** 40.90.139.36, 40.90.139.2


## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [通过 Translator API 调用在防火墙后面翻译](reference/v3-0-translate.md)

