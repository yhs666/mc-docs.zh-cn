---
title: Azure Database for PostgreSQL 中支持的版本
description: 说明 Azure Database for PostgreSQL 中支持的版本。
services: postgresql
author: WenJason
ms.author: v-jay
manager: digimobile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
origin.date: 09/07/2018
ms.date: 10/01/2018
ms.openlocfilehash: 1bd89feeb168720785d13ebae47eefda1516903b
ms.sourcegitcommit: 04071a6ddf4e969464d815214d6fdd9813c5c5a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2018
ms.locfileid: "47426293"
---
# <a name="supported-postgresql-database-versions"></a>支持的 PostgreSQL Database 版本
Microsoft 计划在 Azure Database for PostgreSQL 服务中支持 n-2 版本的 PostgreSQL 引擎，即当前发布的主要版本 (n) 和两个主要版本 (-2)。

Azure Database for PostgreSQL 目前支持以下版本：

## <a name="postgresql-version-104"></a>PostgreSQL 版本 10.4
若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/10/static/release-10-4.html)。

## <a name="postgresql-version-969"></a>PostgreSQL 版本 9.6.9
若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/9.6/static/release-9-6-9.html)。

## <a name="postgresql-version-9513"></a>PostgreSQL 版本 9.5.13
若要了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/9.5/static/release-9-5-13.html)。

## <a name="managing-updates-and-upgrades"></a>管理更新和升级
Azure Database for PostgreSQL 自动管理次要版本更新的修补。 目前不支持主版本升级。 例如，不支持从 PostgreSQL 9.5 升级到 PostgreSQL 9.6。 如果要升级到下一个主版本，请进行[转储并将其还原](./howto-migrate-using-dump-and-restore.md)到使用新引擎版本创建的服务器。

## <a name="next-steps"></a>后续步骤
有关不同 PostgreSQL 扩展支持的信息，请参阅 [PostgreSQL 扩展](concepts-extensions.md)。
