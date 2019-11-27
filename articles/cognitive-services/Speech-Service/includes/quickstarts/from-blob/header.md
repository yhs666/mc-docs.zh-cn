---
title: 快速入门：识别存储在 Blob 存储中的语音 - 语音服务
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 10/28/2019
ms.date: 11/25/2019
ms.author: v-tawe
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 3bde743650804b5f81a28e3467f4ff32a2730453
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389982"
---
在本快速入门中，你将使用[批量听录 REST API](../../../batch-transcription.md) 识别存储在 [SAS Blob](/cognitive-services/speech-service/) 中的语音。 满足几项先决条件后，使用 REST API 识别语音只需几个步骤：
> [!div class="checklist"]
> * 将 JSON 请求发送到语音服务以开始转录语音。
> * 查看听录的状态。
> * 完成后，下载听录结果。