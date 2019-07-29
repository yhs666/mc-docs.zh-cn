---
title: 如何在 Azure Database for MariaDB 中配置服务器参数
description: 本文介绍如何使用 Azure 门户在 Azure Database for MariaDB 中配置 MariaDB 服务器参数。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.topic: conceptual
origin.date: 04/15/2019
ms.date: 07/22/2019
ms.openlocfilehash: 0bcd869a298c8ebc8a8e05109bdb7a4fc8198f25
ms.sourcegitcommit: 021dbf0003a25310a4c8582a998c17729f78ce42
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2019
ms.locfileid: "68331933"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mariadb-by-using-the-azure-portal"></a>如何使用 Azure 门户在 Azure Database for MariaDB 中配置服务器参数

Azure Database for MariaDB 支持配置某些服务器参数。 本文介绍如何使用 Azure 门户配置这些参数。 并非所有服务器参数都可调整。

## <a name="navigate-to-server-parameters-on-azure-portal"></a>在 Azure 门户中导航到“服务器参数”

1. 登录到 Azure 门户，然后定位到 Azure Database for MariaDB 服务器。
2. 在“设置”  部分下，单击“服务器参数”  ，打开 Azure Database for MariaDB 服务器的“服务器参数”页。
![Azure 门户中的服务器参数页](./media/howto-server-parameters/azure-portal-server-parameters.png)
3. 定位需要调整的任何设置。 查看“说明”列  ，了解用途和允许的值。
![枚举下拉按钮](./media/howto-server-parameters/3-toggle_parameter.png)
4. 单击“保存”  ，保存更改。
![保存或放弃更改](./media/howto-server-parameters/4-save_parameters.png)
5. 保存参数的新值后，随时可以通过选择“全部重置为默认设置”，将所有设置还原为默认值。 
![全部重置为默认设置](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>可配置的服务器参数列表

受支持服务器参数的列表还在不断增加。 在 Azure 门户中使用服务器参数选项卡，以根据应用程序要求获取定义并配置服务器参数。

## <a name="non-configurable-server-parameters"></a>不可配置的服务器参数

InnoDB 缓冲池和最大连接数不可配置，因[定价层](concepts-pricing-tiers.md)而定。

|**定价层**| **vCore(s)**|InnoDB 缓冲池 (MB) | 最大连接数 |
|---|---|---|---|
|基本| 1| 1024| 50|
|基本| 2| 2560| 100|
|常规用途| 2| 3584| 300|
|常规用途| 4| 7680| 625|
|常规用途| 8| 15360| 1250|
|常规用途| 16| 31232| 2500|
|常规用途| 32| 62976| 5000|
|常规用途| 64| 125952| 10000|
|内存优化| 2| 7168| 600|
|内存优化| 4| 15360| 1250|
|内存优化| 8| 30720| 2500|
|内存优化| 16| 62464| 5000|
|内存优化| 32| 125952| 10000|

以下附加服务器参数不可在系统中配置：

|**参数**|**固定值**|
| :------------------------ | :-------- |
|基本层中的 innodb_file_per_table|OFF|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512MB|

在 [MariaDB](https://mariadb.com/kb/en/library/xtradbinnodb-server-system-variables/) 中，上表中未列出的其他服务器参数将设置为其 MariaDB 现成默认值。

<!--
## Next steps

- [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md).
-->
