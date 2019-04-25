---
title: 使用 ODBC 连接到 Azure 数据资源管理器
description: 本操作指南介绍如何设置到 Azure 数据资源管理器的 ODBC 连接，然后使用该连接通过 Tableau 将数据可视化。
author: orspod
ms.author: v-biyu
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
origin.date: 02/21/2019
ms.date: 05/01/2019
ms.openlocfilehash: b78536e46ceee6b8ae4ea4c03f6cb831c5312b13
ms.sourcegitcommit: bf3df5d77e5fa66825fe22ca8937930bf45fd201
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59686685"
---
# <a name="connect-to-azure-data-explorer-with-odbc"></a>使用 ODBC 连接到 Azure 数据资源管理器

开放式数据库连接 ([ODBC](https://docs.microsoft.com/zh-cn/sql/odbc/reference/odbc-overview)) 是一种广泛接受的应用程序编程接口 (API)，适用于数据库访问。 使用 ODBC 可从没有专用连接器的应用程序连接到 Azure 数据资源管理器。

在后台，应用程序会在 ODBC 接口中调用函数，这些函数在特定于数据库的模块（称为“驱动程序”）中实现。 Azure 数据资源管理器支持部分 SQL Server 通信协议 ([MS-TDS](https://docs.microsoft.com/zh-cn/azure/kusto/api/tds/))；因此，它可以使用适用于 SQL Server 的 ODBC 驱动程序。

本文介绍如何使用 SQL Server ODBC 驱动程序，目的是让你能够从任何支持 ODBC 的应用程序连接到 Azure 数据资源管理器。 然后，你可以选择从 Tableau 连接到 Azure 数据资源管理器，并从示例群集引入数据。

## <a name="prerequisites"></a>先决条件

需要以下各项来完成这一过程：

* 适用于操作系统的 [Microsoft ODBC Driver for SQL Server 17.2.0.1 或更高版本](https://docs.microsoft.com/zh-cn/sql/connect/odbc/download-odbc-driver-for-sql-server)。

* 若要遵循 Tableau 示例进行操作，则还需：

  * Tableau Desktop，完整版或[试用](https://www.tableau.com/products/desktop/download)版。

  * 包括 StormEvents 示例数据的群集。 有关详细信息，请参阅[快速入门：创建 Azure 数据资源管理器群集和数据库](create-cluster-database-portal.md)和[将示例数据引入到 Azure 数据资源管理器](ingest-sample-data.md)。

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="configure-the-odbc-data-source"></a>配置 ODBC 数据源

按以下步骤操作，使用 ODBC Driver for SQL Server 配置 ODBC 数据源。

1. 在 Windows 中搜索“ODBC 数据源”，打开 ODBC 数据源桌面应用。

1. 选择“设置” （应用程序对象和服务主体对象）。

    ![添加数据源](media/connect-odbc/add-data-source.png)

1. 选择“ODBC Driver 17 for SQL Server”，然后选择“完成”。

    ![选择驱动程序](media/connect-odbc/select-driver.png)

1. 输入连接的名称和说明以及要连接到的群集，然后选择“下一步”。 群集 URL 应该采用 *\<ClusterName\>.\<区域\>.kusto.windows.net* 格式。

    ![选择服务器](media/connect-odbc/select-server.png)

1. 选择“Active Directory 集成”，然后选择“下一步”。

    ![Active Directory 集成](media/connect-odbc/active-directory-integrated.png)

1. 选择包含示例数据的数据库，然后选择“下一步”。

    ![更改默认数据库](media/connect-odbc/change-default-database.png)

1. 在下一屏幕中将所有选项保留为默认设置，然后选择“完成”。

1. 选择“测试数据源”。

    ![测试数据源](media/connect-odbc/test-data-source.png)

1. 验证测试是否成功，然后选择“确定”。 如果测试不成功，请检查在前面的步骤中指定的值，并确保有足够的权限连接到群集。

    ![测试成功](media/connect-odbc/test-succeeded.png)

## <a name="visualize-data-in-tableau-optional"></a>在 Tableau 中可视化数据（可选）

配置完 ODBC 后，即可将示例数据引入 Tableau。

1. 在 Tableau Desktop 的左侧菜单中，选择“其他数据库(ODBC)”。

    ![与 ODBC 连接](media/connect-odbc/connect-odbc.png)

1. 至于“DSN”，请选择为 ODBC 创建的数据源，然后选择“登录”。

    ![ODBC 登录](media/connect-odbc/odbc-sign-in.png)

1. 至于“数据库”，请选择在示例群集上创建的数据库，例如“TestDatabase”。 至于“架构”，请选择“dbo”；至于“表”，请选择“StormEvents”示例表。

    ![选择数据库和表](media/connect-odbc/select-database-table.png)

1. Tableau 现在会显示示例数据的架构。 选择“立即更新”，将数据引入 Tableau。

    ![更新数据](media/connect-odbc/update-data.png)

    将数据引入以后，Tableau 会显示类似于下图的数行数据。

    ![结果集](media/connect-odbc/result-set.png)

1. 现在可以根据你从 Azure 数据资源管理器引入的数据在 Tableau 中创建可视化效果。 有关详细信息，请参阅 [Tableau 学习](https://www.tableau.com/learn)。

## <a name="next-steps"></a>后续步骤

[Azure 数据资源管理器的编写查询](write-queries.md)

[教程：在 Power BI 中可视化 Azure 数据资源管理器中的数据](visualize-power-bi.md)