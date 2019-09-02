---
title: Azure Batch 池删除开始事件 | Microsoft Docs
description: Batch 池删除开始事件参考。
services: batch
author: lingliw
manager: digimobile
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
origin.date: 06/20/2019
ms.date: 05/14/2018
ms.author: v-lingwu
ms.openlocfilehash: 12f1ef9d73a9610f76cbcbe4bfde7a65b74c4880
ms.sourcegitcommit: 13642a99cc524a416b40635f48676bbf5cdcdf3d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2019
ms.locfileid: "70104069"
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