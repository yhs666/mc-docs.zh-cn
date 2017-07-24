---
title: "Azure Batch 池删除开始事件 | Microsoft Docs"
description: "Batch 池删除开始事件参考。"
services: batch
author: alexchen2016
manager: digimobile
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
origin.date: 04/20/2017
ms.date: 07/03/2017
ms.author: v-junlch
ms.openlocfilehash: ebae75a14ad3ed9253b4bfe22cbc3f580367a23e
ms.sourcegitcommit: d5d647d33dba99fabd3a6232d9de0dacb0b57e8f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2017
---
# <a name="pool-delete-start-event"></a>池删除开始事件

 当池删除操作开始时，会发出此事件。 由于池删除是异步事件，预计在删除操作完成后会发出池删除完成事件。

 以下示例显示了池删除开始事件的正文。

```
{
    "id": "myPool1"
}
```

|元素|类型|说明|
|-------------|----------|-----------|
|id|String|池的 id。|

