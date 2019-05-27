---
title: Azure Database for MariaDB 数据复制存储过程
description: 本文介绍了所有用于数据复制的存储过程。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.topic: reference
origin.date: 09/24/2018
ms.date: 05/27/2019
ms.openlocfilehash: 4a4194b10952589d558bad42deefc5885f49aa6d
ms.sourcegitcommit: 60169f39663ae62016f918bdfa223c411e249883
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66173206"
---
# <a name="azure-database-for-mariadb-data-in-replication-stored-procedures"></a>Azure Database for MariaDB 数据复制存储过程

通过数据传入复制，可将数据从本地运行的 MariaDB 服务器、虚拟机中或其他云提供程序托管的数据库服务中同步到 Azure Database for MariaDB 服务。

下面的存储过程用于在主体和副本之间设置或删除数据传入复制。

|**存储过程名称**|**输入参数**|**输出参数**|**用法说明**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|不适用|若要使用 SSL 模式传输数据，请将 CA 证书的上下文传递到 master_ssl_ca 参数中。 </br><br>若要不使用 SSL 模式传输数据，请将空字符串传递到 master_ssl_ca 参数中。|
|mysql.az_replication _start|不适用|不适用|开始复制。|
|mysql.az_replication _stop|不适用|不适用|停止复制。|
|*mysql.az_replication _remove_master*|不适用|不适用|删除主体和副本之间的复制关系。|
|mysql.az_replication_skip_counter|不适用|不适用|跳过一个复制错误。|

若要在 Azure Database for MariaDB 中的主体和副本之间设置数据传入复制，请参阅[如何配置数据传入复制](howto-data-in-replication.md)。
