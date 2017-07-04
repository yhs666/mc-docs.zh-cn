---
title: "池删除开始事件 - Azure | Microsoft Docs"
ms.custom: 
ms.date: 2017-02-01
ms.prod: azure
ms.reviewer: 
ms.service: batch
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: feddca1e-c69c-4257-be44-a36172e77157
caps.latest.revision: "4"
author: tamram
ms.author: v-junlch
manager: timlt
ms.openlocfilehash: 4c044f5b3e75c0522ac839023b30177113e2c1db
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="pool-delete-start-event"></a>池删除开始事件
池删除开始事件日志正文

## <a name="remarks"></a>备注
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
