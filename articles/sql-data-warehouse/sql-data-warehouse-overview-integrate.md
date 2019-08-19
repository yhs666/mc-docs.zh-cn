---
title: 使用 SQL 数据仓库构建集成解决方案 | Microsoft 文档
description: '用于集成 SQL 数据仓库的工具以及提供相应解决方案的合作伙伴。 '
services: sql-data-warehouse
author: WenJason
manager: digimobile
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
origin.date: 04/17/2018
ms.date: 08/19/2019
ms.author: v-jay
ms.reviewer: igorstan
ms.openlocfilehash: 382846c7f542929134074d0b32242807cbdcc424
ms.sourcegitcommit: 52ce0d62ea704b5dd968885523d54a36d5787f2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69544246"
---
# <a name="integrate-other-services-with-sql-data-warehouse"></a>使用 SQL 数据仓库集成其他服务
除了本身的核心功能以外，SQL 数据仓库还允许用户集成 Azure 中的其他许多服务。 这些服务包括：

* Azure 数据工厂
* Azure 流分析

## <a name="azure-data-factory"></a>Azure 数据工厂
Azure 数据工厂为用户提供一个托管平台，用于创建复杂的提取和加载管道。 SQL 数据仓库与 Azure 数据工厂的集成包括：

* **存储过程**：协调 SQL 数据仓库上存储过程的执行。
* **复制**：使用 ADF 将数据移动到 SQL 数据仓库。 实际上，此操作可以使用 ADF 标准数据移动机制或封面下的 PolyBase。 

有关详细信息，请参阅[与 Azure 数据工厂集成](/data-factory/load-azure-sql-data-warehouse?toc=/sql-data-warehouse/toc.json)。

## <a name="azure-stream-analytics"></a>Azure 流分析
Azure 流分析是复杂、完全托管的基础结构，用于处理和使用从 Azure 事件中心生成的事件数据。  通过与 SQL 数据仓库集成，可以有效地处理流数据，并将其与关系数据一起存储以实现更深入、更高级的分析。  

* **作业输出：** 将流分析作业的输出直接发送到 SQL 数据仓库。

有关详细信息，请参阅[与 Azure 流分析集成](sql-data-warehouse-integrate-azure-stream-analytics.md)。


