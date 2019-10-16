---
title: Azure Batch 池删除开始事件 | Microsoft Docs
description: Batch 池删除开始事件参考。
services: batch
author: lingliw
manager: digimobile
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
origin.date: 04/20/2017
ms.date: 06/20/2019
ms.author: v-lingwu
ms.openlocfilehash: 91cb29371c029b3f5cd0befc491b52aa55108e63
ms.sourcegitcommit: 2f2ced6cfaca64989ad6114a6b5bc76700870c1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71329762"
---
# <a name="pool-delete-start-event"></a>池删除开始事件

 当池删除操作开始时，会发出此事件。 由于池删除是异步事件，预计在删除操作完成后会发出池删除完成事件。

 以下示例显示了池删除开始事件的正文。

```
{
    "id": "myPool1"
}
```

|元素|类型|注释|
|-------------|----------|-----------|
|`id`|String|池的 ID。|

<!-- Update_Description: update metedata properties -->