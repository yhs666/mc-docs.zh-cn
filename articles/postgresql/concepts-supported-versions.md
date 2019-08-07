---
title: Azure Database for PostgreSQL - 单一服务器中支持的版本
description: 介绍了 Azure Database for PostgreSQL - 单一服务器中支持的版本。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 06/11/2019
ms.date: 08/05/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: a7f33b7621bbe822123214182ef4a9e9bac4b8b6
ms.sourcegitcommit: 193f49f19c361ac6f49c59045c34da5797ed60ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2019
ms.locfileid: "68732299"
---
# <a name="supported-postgresql-database-versions"></a>支持的 PostgreSQL 数据库版本
Azure 致力于在 Azure Database for PostgreSQL - 单一服务器中支持 n-2 版本的 PostgreSQL 引擎。 这些版本将是 Azure 上的当前主版本 (n) 和之前的两个主版本 (-2)。

Azure Database for PostgreSQL 目前支持以下版本：

## <a name="postgresql-version-112"></a>PostgreSQL 版本 11.2
若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/11/static/release-11-2.html)。

>[!NOTE]
> PostgreSQL 版本 11 以预览版提供。 对使用 Azure 门户创建的支持正在推出，可能尚未在你所在的区域提供。 可以使用 [Azure CLI](quickstart-create-server-database-azure-cli.md) 在任何区域中创建 Postgres 11 服务器。 例如，`az postgres server create -g group -n server -u username -p password -l chinaeast2 --sku-name GP_Gen5_2 --version 11`。

## <a name="postgresql-version-107"></a>PostgreSQL 版本 10.7
若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/10/static/release-10-7.html)。

## <a name="postgresql-version-9612"></a>PostgreSQL 版本 9.6.12
若要详细了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/9.6/static/release-9-6-12.html)。

## <a name="postgresql-version-9516"></a>PostgreSQL 版本 9.5.16
若要了解此次要版本中的改进和修复，请参阅 [PostgreSQL 文档](https://www.postgresql.org/docs/9.5/static/release-9-5-16.html)。

## <a name="managing-updates-and-upgrades"></a>管理更新和升级
Azure Database for PostgreSQL 自动管理次要版本修补程序。 目前不支持主版本升级。 例如，不支持从 PostgreSQL 9.5 升级到 PostgreSQL 9.6。 如果要升级到下一主版本，请创建数据库[转储并将其还原](./howto-migrate-using-dump-and-restore.md)到使用新引擎版本创建的服务器。

> 请注意，在 PostgreSQL 版本 10 之前，[PostgreSQL 版本控制策略](https://www.postgresql.org/support/versioning/)将_主版本_升级视为第一个_或_第二个数字的增加（例如，9.5 到 9.6 视为_主_版本升级）。
> 从版本 10 开始，只有第一个数字的更改才视为主版本升级（例如，10.0 到 10.1 是_次要_版本升级，10 到 11 是_主_版本升级）。

## <a name="next-steps"></a>后续步骤
有关不同 PostgreSQL 扩展支持的信息，请参阅 [PostgreSQL 扩展](concepts-extensions.md)。
