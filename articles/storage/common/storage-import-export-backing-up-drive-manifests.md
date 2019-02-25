---
title: 备份 Azure 导入/导出驱动器清单 | Microsoft Docs
description: 了解如何自动备份 Azure 导入/导出服务的驱动器清单。
author: WenJason
services: storage
ms.service: storage
ms.topic: article
origin.date: 01/23/2017
ms.date: 02/25/2019
ms.author: v-jay
ms.subservice: common
ms.openlocfilehash: 1481333e5416ed6a372ff54f9d2bd6a4b9f835a0
ms.sourcegitcommit: 0fd74557936098811166d0e9148e66b350e5b5fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56665602"
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a>为 Azure 导入/导出作业备份驱动器清单

通过在[放置作业](https://docs.microsoft.com/rest/api/storageimportexport/jobs)或[更新作业属性](https://docs.microsoft.com/rest/api/storageimportexport/jobs) REST API 操作中将 `BackupDriveManifest` 属性设置为 `true`，可以自动将驱动器清单备份到 blob。 默认情况下，不会备份驱动器清单。 驱动器清单备份以块 Blob 的形式存储在与作业关联的存储帐户中的某个容器内。 该容器的名称默认为 `waimportexport`，但可以在调用 `Put Job` 或 `Update Job Properties` 操作时在 `DiagnosticsPath` 属性中指定不同的名称。 备份清单 Blob 按以下格式命名： `waies/jobname_driveid_timestamp_manifest.xml`。

 可以调用 [获取作业](https://docs.microsoft.com/rest/api/storageimportexport/jobs) 操作来检索备份驱动器清单的 URI。 在每个驱动器的 `ManifestUri` 属性中会返回 Blob URI。

## <a name="next-steps"></a>后续步骤

* [使用导入/导出服务 REST API](storage-import-export-using-the-rest-api.md)
<!--Update_Description: update link-->