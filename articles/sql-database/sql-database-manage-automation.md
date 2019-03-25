---
title: 使用 Azure 自动化管理 Azure SQL 数据库 | Microsoft Docs
description: 了解如何使用 Azure 自动化服务来管理大规模的 Azure SQL 数据库。
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: digimobile
origin.date: 12/19/2018
ms.date: 03/25/2019
ms.openlocfilehash: c97419111b394e3fef64355e9cf6d8c29a49f698
ms.sourcegitcommit: 02c8419aea45ad075325f67ccc1ad0698a4878f4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318859"
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a>使用 Azure 自动化管理 Azure SQL 数据库

本指南介绍 Azure 自动化服务，以及如何使用它来简化 Azure SQL 数据库的管理。

## <a name="what-is-azure-automation"></a>什么是 Azure 自动化？

[Azure 自动化](https://www.azure.cn/home/features/automation/)是用于通过流程自动化简化云管理的一项 Azure 服务。 使用 Azure 自动化可以自动完成那些长时间运行、人工操作、易出错和经常重复的任务，从而改善组织的可靠性、效率和价值生成时间。

Azure 自动化提供了高度可靠且高度可用的工作流执行引擎，该引擎可以随着组织的发展根据需求进行扩展。 在 Azure 自动化中，流程可以手动、通过第三方系统或按计划的间隔启动，使任务能够完全根据需求进行。

通过将云管理任务改为由 Azure 自动化自动运行，可以降低运营开销，解放 IT/DevOps 人员，让他们将精力集中在增加企业价值的工作上。

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Azure 自动化如何帮助管理 Azure SQL 数据库？

可以使用 [Azure PowerShell 工具](https://docs.microsoft.com/powershell/azure/overview)中提供的 [Azure SQL 数据库 PowerShell cmdlet](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#sql) 在 Azure 自动化中管理 Azure SQL 数据库。 Azure 自动化现成地提供了这些 Azure SQL 数据库 PowerShell cmdlet，因此，可以在该服务中执行所有 SQL DB 管理任务。 还可以将 Azure 自动化中的这些 cmdlet 与其他 Azure 服务的 cmdlet 搭配使用，以便跨 Azure 服务和第三方系统自动完成复杂的任务。

Azure 自动化还可以通过使用 PowerShell 发出 SQL 命令，与 SQL 服务器直接通信。

[Azure 自动化 Runbook 库](https://azure.microsoft.com/blog/20../../introducing-the-azure-automation-runbook-gallery/)包含产品团队和社区提供的各种 Runbook，以帮助你开始自动管理 Azure SQL 数据库、其他 Azure 服务和第三方系统。 库 Runbook 包括：

- [对 SQL Server 数据库运行 SQL 查询](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
- [按计划纵向缩放（向上或向下）Azure SQL 数据库](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
- [当数据库接近其最大大小时截断 SQL 表](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
- [当 Azure SQL 数据库中的表高度碎片化时为这些表编制索引](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>后续步骤

在了解 Azure 自动化 以及如何使用它来管理 Azure SQL 数据库的基础知识后，请使用以下链接了解有关 Azure 自动化的更多信息。

- [Azure 自动化概述](../automation/automation-intro.md)
- [我的第一个 Runbook](../automation/automation-first-runbook-graphical.md)
- [Azure 自动化：云中的 SQL 代理](https://azure.microsoft.com/blog/20../../azure-automation-your-sql-agent-in-the-cloud/) 