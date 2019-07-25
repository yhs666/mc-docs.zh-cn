---
title: Azure 防火墙服务标记概述
description: 本文是 Azure 防火墙服务标记的概述。
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: article
origin.date: 06/27/2019
ms.date: 07/22/2019
ms.author: v-yeche
ms.openlocfilehash: c68430da796a26356b0d247d5e53cc6189ffda2e
ms.sourcegitcommit: 5fea6210f7456215f75a9b093393390d47c3c78d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68337552"
---
# <a name="azure-firewall-service-tags"></a>Azure 防火墙服务标记

服务标记表示一组 IP 地址前缀，帮助最大程度地降低安全规则创建过程的复杂性。 无法创建自己的服务标记，也无法指定要将哪些 IP 地址包含在标记中。 Azure 会管理服务标记包含的地址前缀，并会在地址发生更改时自动更新服务标记。

Azure 防火墙服务标记可用于网络规则目标字段。 它们可用于代替特定的 IP 地址。

## <a name="supported-service-tags"></a>支持的服务标记

有关可在 Azure 防火墙网络规则中使用的服务标记的列表，请参阅[安全组](../virtual-network/security-overview.md#service-tags)。

## <a name="next-steps"></a>后续步骤

若要详细了解 Azure 防火墙规则，请参阅 [Azure 防火墙规则处理逻辑](rule-processing.md)。

<!-- Update_Description: new article about service tag -->
<!--ms.date: 07/22/2019-->