---
title: 升级 Azure 备份代理
description: 此信息说明为什么应升级 Azure 备份代理，以及应在何处下载升级。
services: backup
cloud: ''
suite: ''
author: markgalioto
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
origin.date: 03/16/2018
ms.date: 04/08/2018
ms.author: v-junlch
ms.openlocfilehash: 400c631d5ae1344de822874acd4417d5c49a4f93
ms.sourcegitcommit: ce691e6877a362d33b5484b9bbf85c93915689a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2018
---
## <a name="upgrade-the-mars-agent"></a>升级 MARS 代理
低于 2.0.9083.0 的 Microsoft Azure 恢复服务 (MARS) 代理版本依赖于 Azure 访问控制服务 (ACS)。 在 2018 年，Azure 将弃用 Azure 访问控制服务 (ACS)。 从 2018 年 3 月 19 日开始，低于 2.0.9083.0 的所有 MARS 代理版本会遇到备份失败。 若要避免或解决备份失败，请[将 MARS 代理升级到最新版本](https://go.microsoft.com/fwlink/?linkid=229525)。 若要确定需要 MARS 代理升级的服务器，请按照[用于升级 Azure 备份代理的备份博客](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/)中的步骤操作。 MARS 代理用于将以下数据类型备份到 Azure：
- 文件 
- 系统状态数据
- 将 System Center DPM 备份到 Azure
- Azure 备份服务器 (MABS)

<!-- ms.date: 04/08/2018 -->