---
title: 如何配置 Azure Database for MySQL 的数据传入复制
description: 演示如何配置 Azure Database for MySQL 的数据传入复制的步骤
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/16/2018
ms.openlocfilehash: 8456a76e4a41a573fb540e12d6c01a0176365d84
ms.sourcegitcommit: 3d17c1b077d5091e223aea472e15fcb526858930
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2018
ms.locfileid: "37873572"
---
# <a name="how-to-configure-azure-database-for-mysql-data-in-replication"></a>如何配置 Azure Database for MySQL 的数据传入复制

> [!NOTE]
> 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql/)。

本文介绍如何配置主服务器和副本服务器，以便在 Azure Database for MySQL 服务中设置数据传入复制。

本文假定你至少已经有一些 MySQL 服务器和数据库的使用经验。

## <a name="create-a-mysql-server-to-be-used-as-replica"></a>创建将要用作副本的 MySQL 服务器

1. 创建新的 Azure Database for MySQL 服务器

创建新的 MySQL 服务器（例如 “replica.database.chinacloudapi.cn”）。 请参阅[使用 Azure 门户创建 Azure Database for MySQL 服务器](quickstart-create-mysql-server-database-using-azure-portal.md)，了解如何创建服务器。 在数据传入复制中，此服务器为“副本”服务器。

> [!Important]
> 此服务器必须在“常规用途”或“内存优化”定价层中创建。

2. 创建相同的用户帐户和相应的特权

用户帐户不会从主服务器复制到副本服务器。 如果打算为用户提供访问副本服务器的权限，则需在这个新创建的 Azure Database for MySQL 服务器上手动创建所有帐户和相应的特权。

## <a name="configure-the-primary-server"></a>配置主服务器

1. 启用二进制日志记录

运行以下命令，查看是否已在主服务器上启用二进制日志记录：

```sql
SHOW VARIABLES LIKE 'log_bin';
```

如果返回的变量 *log_bin* 的值为“ON”，则表明已在服务器上启用二进制日志记录。

如果返回的 *log_bin* 的值为“OFF”，则请编辑 my.cnf 文件，令 *log_bin=ON*，然后重启服务器，使更改生效，以便启用二进制日志记录。

2. 主服务器设置

数据传入复制要求参数 *lower_case_table_names* 在主服务器和副本服务器之间保持一致。 在 Azure Database for MySQL 中，该参数默认为 1。

```sql
SET GLOBAL lower_case_table_names = 1;
```

3. 创建新的复制角色并设置权限

在主服务器上创建配置了复制特权的用户帐户。 这可以通过 SQL 命令或 MySQL Workbench 之类的工具来完成。 考虑是否打算使用 SSL 进行复制，因为这需要在创建用户时指定。 请参阅 MySQL 文档，了解如何在主服务器上[添加用户帐户](https://dev.mysql.com/doc/refman/5.7/en/adding-users.html)。

在下面的命令中，创建的新复制角色可以从任何计算机访问主服务器，不仅仅从托管主服务器的计算机本身进行访问。 这可以通过在创建用户的命令中指定“syncuser@'%'”来完成。 请参阅 MySQL 文档，详细了解如何[指定帐户名称](https://dev.mysql.com/doc/refman/5.7/en/account-names.html)。

### <a name="sql-command"></a>SQL 命令

#### <a name="replication-with-ssl"></a>使用 SSL 进行复制

如果所有用户连接都要求 SSL，请使用以下命令来创建用户：

```sql
CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
```

#### <a name="replication-without-ssl"></a>在不使用 SSL 的情况下进行复制

如果所有用户连接都不要求 SSL，请使用以下命令来创建用户：

```sql
CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
```

### <a name="mysql-workbench"></a>MySQL Workbench

若要在 MySQL Workbench 中创建复制角色，请在“管理”面板中打开“用户和特权”面板。 然后单击“添加帐户”。

![用户和特权](./media/howto-data-in-replication/users-and-privileges.png)

在“登录名称”字段中键入用户名。

![同步用户](./media/howto-data-in-replication/sync-user.png)

单击“管理角色”面板，然后从“全局特权”列表中选择“复制从属实例”。 然后单击“应用”，创建复制角色。

![复制从属实例](./media/howto-data-in-replication/replication-slave.png)

4. 将主服务器设置为只读模式

在开始转储数据库之前，需将服务器置于只读模式。 在只读模式下，主服务器将无法处理任何写事务。 评估对业务的影响，根据需要在非高峰时间计划只读窗口。

```sql
FLUSH TABLES WITH READ LOCK;
SET GLOBAL read_only = ON;
```

5. 获取二进制日志文件名和偏移量

运行“显示主服务器状态”命令，确定当前的二进制日志文件名和偏移量。

```sql
show master status;
```

结果应如下所示。 确保记下此二进制文件名，因为在后面的步骤中会用到它。

![主服务器状态](./media/howto-data-in-replication/masterstatus.png)

## <a name="dump-and-restore-primary-server"></a>转储和还原主服务器

1. 转储主服务器中的所有数据库

可以使用 mysqldump 来转储主服务器中的数据库。 有关详细信息，请参阅[转储和还原](concepts-migrate-dump-restore.md)。 不需转储 MySQL 库和测试库。

2. 将主服务器设置为读/写模式

将数据库转储以后，请将主 MySQL 服务器改回读/写模式。

```sql
SET GLOBAL read_only = OFF;
UNLOCK TABLES;
```

3. 将转储文件还原到新服务器

将转储文件还原到在 Azure Database for MySQL 服务中创建的服务器。 请参阅[转储和还原](concepts-migrate-dump-restore.md)，了解如何将转储文件还原到 MySQL 服务器。 如果转储文件较大，请将它上传到副本服务器所在区域的 Azure 中的虚拟机。 将它从虚拟机还原到 Azure Database for MySQL 服务器。

## <a name="link-primary-and-replica-servers-to-start-data-in-replication"></a>链接主服务器和副本服务器，以启动数据传入复制

1. 设置主服务器

所有数据传入复制功能都是通过存储过程完成的。 可以在[数据传入复制存储过程](reference-data-in-stored-procedures.md)中找到所有过程。 这些存储过程可以在 MySQL shell 或 MySQL Workbench 中运行。

若要链接两个服务器并启动复制，请在 Azure DB for MySQL 服务中登录到目标副本服务器，然后将外部实例设置为主服务器。 为此，请使用 Azure DB for MySQL 服务器上的 *mysql.az_replication_change_primary* 存储过程。