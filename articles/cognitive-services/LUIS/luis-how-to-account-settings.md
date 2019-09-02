---
title: 管理帐户和密钥
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS 帐户信息的两个关键部分是用户帐户和创作密钥。 登录信息在 account.windowsazure.cn 上进行管理。 创作密钥在 LUIS 门户的“设置”页面上进行管理。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/19/19
ms.author: v-lingwu
ms.openlocfilehash: 249bdf064ec2a511ac5356f43db87ef1d4dbfc52
ms.sourcegitcommit: 52ce0d62ea704b5dd968885523d54a36d5787f2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69544352"
---
# <a name="manage-account-and-authoring-key"></a>管理帐户和创作密钥

LUIS 帐户信息的两个关键部分是用户帐户和创作密钥。 登录信息在 [account.windowsazure.cn](https://account.windowsazure.cn) 上进行管理。 创作密钥在 [LUIS](luis-reference-regions.md) 门户的“设置”  页面上进行管理。

## <a name="authoring-key"></a>创作密钥

使用“设置”页面上的这个区域特定的单个创作密钥，可通过 [LUIS](luis-reference-regions.md) 门户和[创作 API](https://aka.ms/luis-authoring-api) 创作所有应用  。 为方便起见，创作密钥每月可执行[有限](luis-boundaries.md)数量的终结点查询。

[![LUIS 设置页](./media/luis-how-to-account-settings/account-settings.png)](./media/luis-how-to-account-settings/account-settings.png#lightbox)

创作密钥可用于你拥有的所有应用以及列为协作者的任何应用。

## <a name="authoring-key-regions"></a>创作密钥区域

创作密钥特定于[创作区域](luis-reference-regions.md#publishing-regions)。 此密钥不可用于其他区域。

## <a name="reset-authoring-key"></a>重置创作密钥

如果创作密钥遭到泄露，请重置密钥。 可在 [LUIS](luis-reference-regions.md) 门户的任意应用中重置此密钥。 如果通过创作 API 创作应用，则需要将 `Ocp-Apim-Subscription-Key` 的值更改为新密钥。

## <a name="delete-account"></a>删除帐户

要了解删除帐户时一并删除了哪些数据，请参阅[数据存储和删除](luis-concept-data-storage.md#accounts)。

## <a name="next-steps"></a>后续步骤

深入了解[创作密钥](luis-concept-keys.md#authoring-key)。





