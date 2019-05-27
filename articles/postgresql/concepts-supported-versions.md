---
title: Azure Database for PostgreSQL - 单一服务器中支持的版本
description: 介绍了 Azure Database for PostgreSQL - 单一服务器中支持的版本。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 5/6/2019
ms.date: 05/20/2019
ms.openlocfilehash: 59f9db16b61f8e4dc0d8f6f56da0beebafb4156b
ms.sourcegitcommit: 11d81f0e4350a72d296e5664c2e5dc7e5f350926
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/16/2019
ms.locfileid: "65731919"
---
# <a name="supported-postgresql-database-versions"></a>支持的 PostgreSQL 数据库版本
Azure 致力于在 Azure Database for PostgreSQL - 单一服务器中支持 n-2 版本的 PostgreSQL 引擎。 这些版本将是 Azure 上的当前主版本 (n) 和之前的两个主版本 (-2)。

Azure Database for PostgreSQL 目前支持以下版本：

## <a name="postgresql-version-107"></a>PostgreSQL 版本 10.7
若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/10/static/release-10-7.html)。

## <a name="postgresql-version-9612"></a>PostgreSQL 版本 9.6.12
若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/9.6/static/release-9-6-12.html)。

## <a name="postgresql-version-9516"></a>PostgreSQL 版本 9.5.16
若要了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/9.5/static/release-9-5-16.html)。

## <a name="managing-updates-and-upgrades"></a>管理更新和升级
Azure Database for PostgreSQL 自动管理次要版本修补程序。 目前不支持主版本升级。 例如，不支持从 PostgreSQL 9.5 升级到 PostgreSQL 9.6。 如果要升级到下一主版本，请创建数据库[转储并将其还原](./howto-migrate-using-dump-and-restore.md)到使用新引擎版本创建的服务器。

## <a name="next-steps"></a>后续步骤
有关不同 PostgreSQL 扩展支持的信息，请参阅 [PostgreSQL 扩展](concepts-extensions.md)。
