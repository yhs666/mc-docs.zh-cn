---
title: Azure Database for PostgreSQL - 单一服务器中支持的版本
description: 介绍了 Azure Database for PostgreSQL - 单一服务器中支持的版本。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 10/25/2019
ms.date: 11/18/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: bf5d7da6e3123fac3cdb1220d991f72313e52b64
ms.sourcegitcommit: c863b31d8ead7e5023671cf9b58415542d9fec9c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74020826"
---
# <a name="supported-postgresql-database-versions"></a>支持的 PostgreSQL 数据库版本
Azure 致力于在 Azure Database for PostgreSQL - 单一服务器中支持 n-2 版本的 PostgreSQL 引擎。 这些版本将是 Azure 上的当前主版本 (n) 和之前的两个主版本 (-2)。

Azure Database for PostgreSQL 目前支持以下主版本：

## <a name="postgresql-version-11"></a>PostgreSQL 版本 11
当前次要版本为 11.5。 若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/11/static/release-11-5.html)。

## <a name="postgresql-version-10"></a>PostgreSQL 版本 10
当前次要版本为 10.10。 若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/10/static/release-10-10.html)。

## <a name="postgresql-version-96"></a>PostgreSQL 版本 9.6
当前次要版本为 9.6.15。 若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/9.6/static/release-9-6-15.html)。

## <a name="postgresql-version-95"></a>PostgreSQL 版本 9.5
当前次要版本为 9.5.19。 若要了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/9.5/static/release-9-5-19.html)。

## <a name="managing-upgrades"></a>管理升级
Azure Database for PostgreSQL 自动管理次要版本升级。 

不支持自动主版本升级。 例如，没有从 PostgreSQL 9.5 到 PostgreSQL 9.6 的自动升级。 若要升级到下一主版本，请[创建数据库转储并还原](./howto-migrate-using-dump-and-restore.md)到使用新引擎版本创建的服务器。

### <a name="version-syntax"></a>版本语法
在 PostgreSQL 版本 10 之前，[PostgreSQL 版本控制策略](https://www.postgresql.org/support/versioning/)将_主版本_升级视为第一个_或_第二个数字的增加。 例如，9.5 到 9.6 的升级视为_主_版本升级。 从版本 10 开始，只有第一个数字更改才视为主版本升级。 例如，10.0 到 10.1 的升级是_次要_版本升级。 版本 10 到 11 的升级是_主_版本升级。

## <a name="next-steps"></a>后续步骤
有关支持的 PostgreSQL 扩展的信息，请参阅[扩展文档](concepts-extensions.md)。
