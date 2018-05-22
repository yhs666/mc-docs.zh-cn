---
title: Azure Batch 池删除开始事件 | Microsoft Docs
description: Batch 池删除开始事件参考。
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
origin.date: 04/20/2017
ms.date: 05/14/2018
ms.author: v-junlch
ms.openlocfilehash: 6f42f4d0ce77f49cd0117982b7f06afbb83f9106
ms.sourcegitcommit: c3084384ec9b4d313f4cf378632a27d1668d6a6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2018
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
|id|String|池的 id。|

<!-- Update_Description: update metedata properties -->