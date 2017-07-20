---
title: "Azure 导入/导出工具的导入作业命令快速参考 | Azure"
description: "导入作业常用的 Azure 导入/导出工具命令参考"
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/15/2017
ms.date: 02/24/2017
ms.author: v-johch
ms.openlocfilehash: 050338182283eee75b340b2d8a868771e042b78e
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>导入作业的常用命令快速参考

本文提供一些常用命令的快速参考。 有关详细用法，请参阅[为导入作业准备硬盘驱动器](./storage-import-export-tool-preparing-hard-drives-import.md)。

## <a name="import-job-quick-reference"></a>导入作业快速参考

对于第一个会话：

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

第二个会话：

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /DataSet:dataset-2.csv
```

中止最新会话：

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /AbortSession
```

恢复最近中断的会话：

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /ResumeSession
```

将驱动器添加到最新的会话：

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /AdditionalDriveSet:driveset-2.csv
```

## <a name="next-steps"></a>后续步骤

[为导入作业准备硬盘驱动器的示例工作流](./storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)