---
title: "为 Azure SQL 数据库启用自动优化 | Azure"
description: "可以轻松地在 Azure SQL 数据库中启用自动优化。"
services: sql-database
documentationcenter: 
author: hayley244
manager: digimobile
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
origin.date: 06/05/2016
ms.date: 07/03/2017
ms.author: v-johch
ms.openlocfilehash: 9af4601bd3a64851d6cdb791e8125585bd6b4e74
ms.sourcegitcommit: 73b1d0f7686dea85647ef194111528c83dbec03b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2017
---
<a id="enable-automatic-tuning" class="xliff"></a>

# 启用自动优化

Azure SQL 数据库是一种自动托管的数据服务，它会不断监视查询并识别你可以执行的操作，以提高工作负荷的性能。 可以查看建议并手动应用这些建议，或者让 Azure SQL 数据库自动应用纠正措施 - 这称为**自动优化模式**。 可以在服务器或数据库级别启用自动优化。

<a id="enable-automatic-tuning-on-server" class="xliff"></a>

## 在服务器上启用自动优化

若要在 Azure SQL 数据库服务器上启用自动优化，请在 Azure 门户中导航到该服务器，然后在菜单中选择“自动优化”。 选择要启用的自动优化选项，然后选择“应用”：

![服务器](./media/sql-database-automatic-tuning-enable/server.png)

服务器上的自动优化选项将应用到服务器上的所有数据库。 默认情况下，所有数据库将从其父服务器继承配置，但可以针对每个数据库单独覆盖或指定此行为。

<a id="configure-automatic-tuning-on-database" class="xliff"></a>

## 在数据库上配置自动调优

使用 Azure 门户可以单独在每个数据库中指定自动优化配置。

> [!NOTE]
> 一般的建议是在服务器级别管理自动优化配置，以便可以在每个数据库上自动应用相同的配置设置。 如果某个数据库与同一服务器上的其他数据库不同，请在该数据库中单独配置自动优化。
>

若要在单个数据库中启用自动优化，请在 Azure 门户中导航到该数据库，然后选择“自动优化”。 可以通过选中相应的复选框将单个数据库配置为从父数据库继承设置，或者单独为某个数据库指定配置。

![数据库](./media/sql-database-automatic-tuning-enable/database.png)

选择相应的配置后，请单击“应用”。

<a id="next-steps" class="xliff"></a>

## 后续步骤
* 请参阅[性能建议](sql-database-advisor.md)，获取 Azure SQL 数据库性能建议的概述。
* 请参阅[查询性能见解](sql-database-query-performance.md)，了解排名靠前的查询的性能影响。