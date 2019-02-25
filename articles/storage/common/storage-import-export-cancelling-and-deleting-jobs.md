---
title: 取消和删除 Azure 导入/导出作业 | Microsoft Docs
description: 了解如何取消和删除 Azure 导入/导出服务的作业。
author: WenJason
services: storage
ms.service: storage
ms.topic: article
origin.date: 01/23/2017
ms.date: 02/25/2019
ms.author: v-jay
ms.subservice: common
ms.openlocfilehash: 532bd97a2f1806e43b7ff5a762e2ebc7443196b6
ms.sourcegitcommit: 0fd74557936098811166d0e9148e66b350e5b5fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56665366"
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>取消和删除 Azure 导入/导出作业

 若要在作业处于 `Packaging` 状态之前请求取消该作业，请调用[更新作业属性](https://docs.microsoft.com/rest/api/storageimportexport/jobs)操作并将 `CancelRequested` 元素设置为 `true`。 系统会尽最大努力取消该作业。 如果驱动器正在传输数据，则即使在请求取消后，数据也仍可能继续传输。

 已取消的作业将转为 `Completed` 状态并保留 90 天，届时它将被删除。

 若要删除某个作业，请在传送该作业之前（即，在该作业处于 `Creating` 状态时）调用[删除作业](https://docs.microsoft.com/rest/api/storageimportexport/jobs)操作。 也可以在作业处于 `Completed` 状态时将其删除。 删除某个作业后，不再能够通过 REST API 或 Azure 门户访问其信息和状态。

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>后续步骤

* [使用导入/导出服务 REST API](storage-import-export-using-the-rest-api.md)
<!--Update_Description: wording update-->