---
title: Azure Database for MySQL 数据复制存储过程
description: Azure Database for MySQL 数据复制存储过程
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/16/2018
ms.openlocfilehash: 8e39b4f07fb61b4ca59193636caf6ea469c05a12
ms.sourcegitcommit: 044f3fc3e5db32f863f9e6fe1f1257c745cbb928
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36270090"
---
# <a name="azure-database-for-mysql-data-in-replication-stored-procedures"></a>Azure Database for MySQL 数据复制存储过程

> [!NOTE] 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql/)。

借助数据复制，可以将本地运行的 MySQL 服务器、虚拟机或其他云提供程序托管的数据库服务中的数据同步到 Azure Database for MySQL 服务。

下面的存储过程用于在主体和副本之间设置或删除数据复制。

> [!div class="mx-tableFixed"]

|     |     |     |     |
|-----|-----|-----|-----|
|**存储过程名称**|**输入参数**|**输出参数**|**用法说明**|
|mysql.az_replication_change_primary|master_host；master_user；master_password；master_port；master_log_file；master_log_pos；master_ssl_ca|不适用|若要使用 SSL 模式传输数据，请将 CA 证书的上下文传递到 master_ssl_ca 参数中。 若要不使用 SSL 模式传输数据，请将空字符串传递到 master_ssl_ca 参数中。|
|mysql.az_replication _start|不适用|不适用|开始复制。|
|mysql.az_replication _stop|不适用|不适用|停止复制。|
|mysql.az_replication _remove_primary|不适用|不适用|删除主体和副本之间的复制关系。|
|mysql.az_replication_skip_counter|不适用|不适用|跳过一个复制错误。|
|     |     |     |     |

若要在主体和 Azure Database for MySQL 副本之间设置数据复制，请参阅[如何配置数据复制](howto-data-in-replication.md)。