---
title: 列出所有 Azure 导入/导出作业 | Microsoft 文档
description: 了解如何列出订阅中的所有 Azure 导入/导出服务作业。
author: WenJason
services: storage
ms.service: storage
ms.topic: article
origin.date: 01/23/2017
ms.date: 02/25/2019
ms.author: v-jay
ms.subservice: common
ms.openlocfilehash: 16b58dfbaa5fff962a19557359c2c998f4c41819
ms.sourcegitcommit: 0fd74557936098811166d0e9148e66b350e5b5fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56665473"
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a>枚举 Azure 导入/导出服务中的作业
若要枚举某个订阅中的所有作业，请调用 [列出作业](https://docs.microsoft.com/rest/api/storageimportexport/jobs) 操作。 `List Jobs` 返回作业列表以及以下属性：

-   作业的类型（导入或导出）

-   当前作业状态

-   作业的关联存储帐户

## <a name="next-steps"></a>后续步骤

* [使用导入/导出服务 REST API](storage-import-export-using-the-rest-api.md)