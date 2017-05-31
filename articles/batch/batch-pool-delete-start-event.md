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
caps.latest.revision: 4
author: tamram
ms.author: tamram
manager: timlt
ms.translationtype: Human Translation
ms.sourcegitcommit: 457fc748a9a2d66d7a2906b988e127b09ee11e18
ms.openlocfilehash: 5975da6c68ed5c14d0210723e14a5da76d00fa1d
ms.contentlocale: zh-cn
ms.lasthandoff: 05/05/2017

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


