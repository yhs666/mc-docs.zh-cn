---
title: Azure Database for MariaDB 管理存储过程
description: 了解 Azure Database for MySQL 中的哪些存储过程可用于帮助你配置数据传入复制、设置时区和终止查询。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.topic: conceptual
origin.date: 09/20/2019
ms.date: 11/04/2019
ms.openlocfilehash: ff109537168e85d5286f02a0170fa6147e5c9591
ms.sourcegitcommit: f643ddf75a3178c37428b75be147c9383384a816
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "73191603"
---
# <a name="azure-database-for-mariadb-management-stored-procedures"></a>Azure Database for MariaDB 管理存储过程

Azure Database for MariaDB 服务器上提供了可帮助管理 MariaDB 服务器的存储过程。 这包括管理服务器的连接、查询以及设置数据传入复制。  

## <a name="data-in-replication-stored-procedures"></a>数据传入复制存储过程

通过数据传入复制，可将数据从本地运行的 MariaDB 服务器、虚拟机中或其他云提供程序托管的数据库服务中同步到 Azure Database for MariaDB 服务。

下面的存储过程用于在主体和副本之间设置或删除数据传入复制。

|**存储过程名称**|**输入参数**|**输出参数**|**用法说明**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|不适用|若要使用 SSL 模式传输数据，请将 CA 证书的上下文传递到 master_ssl_ca 参数中。 </br><br>若要不使用 SSL 模式传输数据，请将空字符串传递到 master_ssl_ca 参数中。|
|mysql.az_replication _start |不适用|不适用|开始复制。|
|mysql.az_replication _stop |不适用|不适用|停止复制。|
|*mysql.az_replication _remove_master*|不适用|不适用|删除主体和副本之间的复制关系。|
|mysql.az_replication_skip_counter |不适用|不适用|跳过一个复制错误。|

若要在 Azure Database for MariaDB 中的主体和副本之间设置数据传入复制，请参阅[如何配置数据传入复制](howto-data-in-replication.md)。

## <a name="other-stored-procedures"></a>其他存储过程

Azure Database for MariaDB 中提供了以下用于管理服务器的存储过程。

|**存储过程名称**|**输入参数**|**输出参数**|**用法说明**|
|-----|-----|-----|-----|
|*mysql.az_kill*|processlist_id|不适用|等效于 [`KILL CONNECTION`](https://dev.mysql.com/doc/refman/8.0/en/kill.html) 命令。 在终止连接正在执行的任何语句之后，将终止与提供的 process list_id 关联的连接。|
|*mysql.az_kill_query*|processlist_id|不适用|等效于 [`KILL QUERY`](https://dev.mysql.com/doc/refman/8.0/en/kill.html) 命令。 将终止连接当前正在执行的语句。 使连接本身保持活动状态。|
|*mysql.az_load_timezone*|不适用|不适用|加载时区表以允许将 `time_zone` 参数设置为命名值（例如， “US/Pacific”）。|

## <a name="next-steps"></a>后续步骤
- 了解如何设置[数据传入复制](howto-data-in-replication.md)
- 了解如何使用[时区表](howto-server-parameters.md#working-with-the-time-zone-parameter)