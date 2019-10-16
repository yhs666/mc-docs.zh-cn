---
title: 安装语音容器
titleSuffix: Azure Cognitive Services
description: 详细介绍了语音伞形 Helm 图配置选项。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 06/26/2019
ms.date: 09/23/2019
ms.author: v-tawe
ms.openlocfilehash: fdfbade4584b37efe8938b063b6595f4d9b1ee62
ms.sourcegitcommit: c72fba1cacef1444eb12e828161ad103da338bb1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2019
ms.locfileid: "71696249"
---
### <a name="speech-umbrella-chart"></a>语音（伞形图）

顶级“伞形”图中的值会替代相应的子图值。 因此，应在此处添加所有本地自定义值。

|参数|说明|默认|
| -- | -- | -- | -- |
| `textToSpeech.enabled` | 是否启用了文本转语音服务。 | `true` |
| `textToSpeech.verification.enabled` | 是否为**文本转语音**服务启用了 `helm test` 功能。 | `true` |
| `textToSpeech.verification.image.registry` | 供 `helm test` 用来测试**文本转语音**服务的 docker 映像存储库。 Helm 在群集内创建单独的 Pod 用于测试，并从该注册表拉取 *test-use* 映像。 | `docker.io` |
| `textToSpeech.verification.image.repository` | 供 `helm test` 用来测试**文本转语音**服务的 docker 映像存储库。 Helm 测试 Pod 使用此存储库拉取 *test-use* 映像。 | `antsu/on-prem-client` |
| `textToSpeech.verification.image.tag` | 与 `helm test` 一起用于**文本转语音**服务的 docker 映像标记。 Helm 测试 Pod 使用此标记拉取 *test-use* 映像。 | `latest` |
| `textToSpeech.verification.image.pullByHash` | 是否通过哈希方法拉取 *test-use* docker 映像。 如果为 `true`，则 `textToSpeech.verification.image.hash` 应随有效的映像哈希值一起添加。 | `false` |
| `textToSpeech.verification.image.arguments` | 用于执行 *test-use* docker 映像的参数。 Helm 测试 Pod 在运行 `helm test` 时将这些参数传递给容器。 | `"./text-to-speech-client"`<br/> `"--input='What's the weather like'"` <br/> `"--host=$(TEXT_TO_SPEECH_HOST)"`<br/>`"--port=$(TEXT_TO_SPEECH_PORT)"` |