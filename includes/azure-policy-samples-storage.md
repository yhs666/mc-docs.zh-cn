---
title: include 文件
description: include 文件
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 08/21/2019
ms.date: 11/12/2018
ms.author: v-biyu
ms.custom: include file
ms.openlocfilehash: 6f2bf276e7abcf05ff76d7eb391164b9660f9c73
ms.sourcegitcommit: 0bfa3c800b03216b89c0461e0fdaad0630200b2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72526633"
---
## <a name="storage"></a>存储

|  |  |
|---------|---------|
| [允许的存储帐户和虚拟网络的 SKU](../articles/governance/policy/samples/allowed-skus-storage.md) | 要求存储帐户和虚拟机使用已批准的 SKU。 使用内置策略以确保使用已批准的 SKU。 指定已批准的虚拟机 SKU 数组和已批准的存储帐户 SKU 数组。 |
| [允许的存储帐户 SKU](../articles/governance/policy/samples/allowed-stor-acct-skus.md) | 要求存储帐户使用批准的 SKU。 指定一个已批准的 SKU 的数组。 |
| [拒绝存储帐户的冷访问层](../articles/governance/policy/samples/deny-cool-access-tiering.md) | 禁止为 blob 存储帐户使用冷访问层。  |
| [确保 https 流量仅限于存储帐户](../articles/governance/policy/samples/ensure-https-stor-acct.md) | 要求存储帐户使用 HTTPS 流量。  |
| [确保存储文件加密](../articles/governance/policy/samples/ensure-store-file-enc.md) | 要求对存储帐户启用文件加密。  |
| [需要存储帐户加密](../articles/governance/policy/samples/req-store-acct-enc.md) | 要求存储帐户使用 blob 加密。  |