---
title: Azure 数据资源管理器数据可视化
description: 了解可视化 Azure 数据资源管理器数据的不同方式
services: data-explorer
author: orspod
ms.author: v-biyu
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
origin.date: 06/03/2019
ms.date: 07/22/2019
ms.openlocfilehash: 12e740fe59d9d0c91f2a6c14eb478ee963bd68c1
ms.sourcegitcommit: ea5dc30371bc63836b3cfa665cc64206884d2b4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717355"
---
# <a name="data-visualization-with-azure-data-explorer"></a>使用 Azure 数据资源管理器进行数据可视化 

Azure 数据资源管理器是一项适用于日志和遥测数据的快速且高度可缩放的数据探索服务，可以用来针对大量数据构建复杂的分析解决方案。 Azure 数据资源管理器集成各种可视化工具，因此可以用来可视化数据，并在组织内共享结果。 该数据可以转换成能够对业务造成影响的可操作见解。

数据可视化和报告是数据分析过程中的关键步骤。 Azure 数据资源管理器支持许多项 BI 服务，因此你可以使用最适合自己方案和预算的一项。

* Azure 数据资源管理器可视化：[`render operator`](https://docs.microsoft.com/zh-cn/azure/kusto/query/renderoperator) 使用 Kusto 查询语言，提供各种用于描述查询结果的可视化类型。 查询可视化用于异常情况检测和预测、机器学习等。

* [Power BI](https://powerbi.microsoft.com)：Azure 数据资源管理器提供使用各种方法连接到 Power BI 的功能： 

  * [内置的本机 Power BI 连接器](/data-explorer/power-bi-connector)

  * [将查询从 Azure 数据资源管理器导入 Power BI](/data-explorer/power-bi-imported-query)
 
  * [SQL 查询](/data-explorer/power-bi-sql-query)。

* [Microsoft Excel](https://products.office.com/excel)：Azure 数据资源管理器提供使用内置本机 Excel 连接器连接到 Excel 的功能，也可以将查询从 Azure 数据资源管理器导入 Excel。

* [Grafana](https://grafana.com)：Grafana 提供一个 Azure 数据资源管理器插件，用于可视化 Azure 数据资源管理器中的数据。 请[将 Azure 数据资源管理器设置为 Grafana 的数据源，然后将数据可视化](/data-explorer/grafana)

* [Sisense](https://www.sisense.com)：Azure 数据资源管理器提供使用 JDBC 连接器连接到 Sisense 的功能。 请[将 Azure 数据资源管理器设置为 Sisense 的数据源，然后将数据可视化](/data-explorer/sisense)。

* [Tableau](https://www.tableau.com)：Azure 数据资源管理器提供使用 [ODBC 连接器](/data-explorer/connect-odbc)连接到 Tableau 的功能，并可在 Tableau 中将数据可视化。

* [Qlik](https://www.qlik.com)：Azure 数据资源管理器提供使用 [ODBC 连接器](/data-explorer/connect-odbc)连接到 Qlik 的功能。