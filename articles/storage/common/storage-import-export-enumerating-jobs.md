---
title: "列出所有 Azure 导入/导出作业 | Microsoft 文档"
description: "了解如何列出订阅中的所有 Azure 导入/导出服务作业。"
author: hayley244
manager: digimobile
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f2e619be-1bbd-4a54-9472-9e2f70a83b64
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/23/2017
ms.date: 08/28/2017
ms.author: v-haiqya
ms.openlocfilehash: 2e2d1d992418d06b27892a35085e972514661dc2
ms.sourcegitcommit: 0f2694b659ec117cee0110f6e8554d96ee3acae8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a>枚举 Azure 导入/导出服务中的作业
若要枚举某个订阅中的所有作业，请调用 [列出作业](https://docs.microsoft.com/rest/api/storageimportexport/jobs#Jobs_List) 操作。 
            `List Jobs` 返回作业列表及以下属性：

-   作业的类型（导入或导出）

-   当前作业状态

-   作业的关联存储帐户

## <a name="next-steps"></a>后续步骤

* [使用导入/导出服务 REST API](storage-import-export-using-the-rest-api.md)