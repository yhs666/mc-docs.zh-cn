---
title: "使用 Azure 导入/导出工具 | Azure"
description: "了解如何使用导入/导出工具为导入作业准备硬盘驱动器、修复导入作业或导出作业。"
author: hayley244
manager: digimobile
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f77535bb-d577-438a-bdd3-e15a82e0c543
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/15/2017
ms.date: 08/28/2017
ms.author: v-haiqya
ms.openlocfilehash: b8fcedaa1787f0dc9cdad8b189649457ee707669
ms.sourcegitcommit: 0f2694b659ec117cee0110f6e8554d96ee3acae8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="using-the-azure-importexport-tool"></a>使用 Azure 导入/导出工具 

使用 Azure 导入/导出工具 (WAImportExport.exe) 可以创建和管理 Azure 导入/导出服务的作业，以便将大量数据传入或传出 Azure Blob 存储。

本文档适用于最新版本的 Azure 导入/导出工具。 有关该工具的 v1 版本的使用信息，请参阅[使用 Azure 导入/导出工具 v1](storage-import-export-tool-how-to-v1.md)。

以下文章介绍了如何使用该工具实现以下目的：  

- 安装和设置导入/导出工具。
- 为作业准备硬盘驱动器，以便将数据从驱动器导入 Azure Blob 存储。
- 使用复制日志文件查看作业状态。 
- 修复导入作业。 
- 修复导出作业。 
- 排查在使用 Azure 导入/导出工具的过程中遇到的问题。 

## <a name="next-steps"></a>后续步骤

* [设置 WAImportExport 工具](storage-import-export-tool-setup.md)
<!--Update_Description: wording update-->