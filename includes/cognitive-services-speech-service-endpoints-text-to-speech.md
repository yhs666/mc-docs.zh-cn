---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
origin.date: 05/06/2019
ms.date: 09/24/2019
ms.author: v-tawe
ms.openlocfilehash: 8e9fa746fb99f6bb0b9c0c329765556f8a36fd76
ms.sourcegitcommit: 73a8bff422741faeb19093467e0a2a608cb896e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2019
ms.locfileid: "71306813"
---
### <a name="standard-and-sneural-voices"></a>标准语音和神经语音

使用下表按区域/终结点确定标准语音和神经语音的可用性：

| 区域 | 终结点 | 标准语音 | 神经语音 |
|--------|----------|-----------------|---------------|
| 中国东部 | `https://chinaeast.tts.speech.chinacloudapi.cn/cognitiveservices/v1` | 是 | 否 |
| 中国东部 2 | `https://chinaeast2.tts.speech.chinacloudapi.cn/cognitiveservices/v1` | 是 | 否 |
| 中国北部 | `https://chinanorth.tts.speech.chinacloudapi.cn/cognitiveservices/v1` | 是 | 否 |
| 中国北部 2 | `https://chinanorth2.tts.speech.chinacloudapi.cn/cognitiveservices/v1` | 是 | 否 |
### <a name="custom-voices"></a>自定义语音

如果已经创建了自定义语音字体，请使用已创建的终结点。 还可以使用下面列出的终结点，并将 `{deploymentId}` 替换为语音模型的部署 ID。

| 区域 | 终结点 |
|--------|----------|
| 中国东部 | `https://chinaeast.tts.speech.chinacloudapi.cn/cognitiveservices/v1?deploymentId={deploymentId}` |
| 中国东部 2 | `https://chinaeast2.tts.speech.chinacloudapi.cn/cognitiveservices/v1?deploymentId={deploymentId}` |
| 中国北部 | `https://chinanorth.tts.speech.chinacloudapi.cn/cognitiveservices/v1?deploymentId={deploymentId}` |
| 中国北部 2 | `https://chinanorth2.tts.speech.chinacloudapi.cn/cognitiveservices/v1?deploymentId={deploymentId}` |
