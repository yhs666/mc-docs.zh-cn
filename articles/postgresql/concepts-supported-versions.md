---
title: Azure Database for PostgreSQL 中支持的版本
description: 说明 Azure Database for PostgreSQL 中支持的版本。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 11/12/2018
ms.date: 12/31/2018
ms.openlocfilehash: 180b4bf5d0114bfc258b08006f21a0f6d4ef0861
ms.sourcegitcommit: e96e0c91b8c3c5737243f986519104041424ddd5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806112"
---
# <a name="supported-postgresql-database-versions"></a>支持的 PostgreSQL 数据库版本
Azure 致力于在 Azure Database for PostgreSQL 服务中支持 n-2 版本的 PostgreSQL 引擎。 这些版本将是 Azure 上的当前主版本 (n) 和之前的两个主版本 (-2)。

Azure Database for PostgreSQL 目前支持以下版本：

## <a name="postgresql-version-105"></a>PostgreSQL 版本 10.5
若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/10/static/release-10-5.html)。

## <a name="postgresql-version-9610"></a>PostgreSQL 版本 9.6.10
若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/9.6/static/release-9-6-10.html)。

## <a name="postgresql-version-9514"></a>PostgreSQL 版本 9.5.14
若要了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/9.5/static/release-9-5-14.html)。

## <a name="managing-updates-and-upgrades"></a>管理更新和升级
Azure Database for PostgreSQL 自动管理次要版本修补程序。 目前不支持主版本升级。 例如，不支持从 PostgreSQL 9.5 升级到 PostgreSQL 9.6。 如果要升级到下一主版本，请创建数据库[转储并将其还原](./howto-migrate-using-dump-and-restore.md)到使用新引擎版本创建的服务器。

## <a name="next-steps"></a>后续步骤
有关不同 PostgreSQL 扩展支持的信息，请参阅 [PostgreSQL 扩展](concepts-extensions.md)。
