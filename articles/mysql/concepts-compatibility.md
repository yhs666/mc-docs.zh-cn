---
title: MySQL 驱动程序和管理工具兼容性
description: 本文介绍与 Azure Database for MySQL 兼容的 MySQL 驱动程序和管理工具。
services: mysql
author: WenJason
ms.author: v-jay
editor: jasonwhowell
manager: digimobile
ms.service: mysql
ms.topic: article
origin.date: 02/28/2018
ms.date: 12/03/2018
ms.openlocfilehash: 21154f2b972178d1d4a36e39347c42095b8fe4df
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52672993"
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>与 Azure Database for MySQL 兼容的 MySQL 驱动程序和管理工具
> [!NOTE]
> 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql-database-on-azure/)。

本文介绍与 Azure Database for MySQL 兼容的驱动程序和管理工具。

## <a name="mysql-drivers"></a>MySQL 驱动程序
Azure Database for MySQL 使用世界上最常用的 MySQL 数据库社区版。 因此，它与多种编程语言和驱动程序兼容。 目标是支持三个最新版本的 MySQL 驱动程序，并且与来自开源社区的创建者共同努力，不断改进 MySQL 驱动程序的功能和可用性。 下表提供了已测试并确认与 Azure Database for MySQL 5.6 和 5.7 兼容的驱动程序列表：

| **驱动程序** | **链接** | **兼容版本** | **不兼容版本** | **说明** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | https://secure.php.net/downloads.php | 5.5 5.6 7.x | 5.3 | 对于 PHP 7.0 与 SSL MySQLi 的连接，请在连接字符串中添加 MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT。 <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO 设置：```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` 选项为 false。|
| .Net | [GitHub 上的 MySqlConnector](https://github.com/mysql-net/MySqlConnector) <br> [来自 Nuget 的安装包](https://www.nuget.org/packages/MySqlConnector/) | 0.27 及以上版本 | 0.26.5 及以下版本 | |
| Nodejs |  [GitHub 上的 MySQLjs](https://github.com/mysqljs/mysql/releases) <br> 来自 NPM 的安装包：<br> 从 NPM 运行 `npm install mysql` | 2.15 | 2.14.1 及以下版本 | |
| 前往 | https://github.com/go-sql-driver/mysql/releases | 1.3 | 1.2 及以下版本 | 在连接字符串中使用 allowNativePasswords=true |
| Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3、2.0、2.1、2.2 | 1.2.2 及以下版本 | |
| Java | https://downloads.mariadb.org/connector-java/ | 2.1 2.0 1.6 | 1.5.5 及以下版本 | |

## <a name="management-tools"></a>管理工具
兼容性优势也适用于数据库管理工具。 只要数据库操作在用户权限范围内，现有工具应继续与 Azure Database for MySQL 配合使用。 下表列出了已测试并确认与 Azure Database for MySQL 5.6 和 5.7 兼容的三种常用数据库管理工具：

|                                     | **MySQL Workbench 6.x 及以上版本** | **Navicat 12** | **PHPMyAdmin 4.x 及以上版本** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| 创建、更新、读取、写入、删除 | X | X | X |
| SSL 连接 | X | X | X |
| SQL 查询自动完成 | X | X |  |
| 导入和导出数据 | X | X | X |
| 导出为多种格式 | X | X | X |
| 备份和还原 |  | X |  |
| 显示服务器参数 | X | X | X |
| 显示客户端连接 | X | X | X |
