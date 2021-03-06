---
title: Azure SQL 弹性缩放常见问题 | Microsoft Docs
description: 有关 Azure SQL 数据库弹性缩放的常见问题。
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: digimobile
origin.date: 01/25/2019
ms.date: 02/25/2019
ms.openlocfilehash: 04277d99a02958bc781c0415249a9c2365f7e6d2
ms.sourcegitcommit: 5ea744a50dae041d862425d67548a288757e63d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56663626"
---
# <a name="elastic-database-tools-frequently-asked-questions-faq"></a>弹性数据库工具常见问题解答 (FAQ)

## <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>如果我的分片只有单个租户并且我没有分片键，该如何为架构信息填充分片键

架构信息对象仅用于“拆分/合并”方案。 如果某个应用程序本质上是单租户，那么它则不需要“拆分/合并”工具，因此无需填充架构信息对象。

## <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>在已预配了一个数据库，并且已安装了分片映射管理器的情况下，应如何将此新数据库注册为分片？

请参阅[使用弹性数据库客户端库将分片添加到应用程序](sql-database-elastic-scale-add-a-shard.md)。

## <a name="how-much-do-elastic-database-tools-cost"></a>弹性数据库工具的费用如何

使用弹性数据库客户端库不会产生任何费用。 只有为分片使用的 Azure SQL 数据库和分片映射管理器，以及为“拆分/合并”工具预配的 Web/辅助角色才会产生费用。

## <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>为什么当我从另一台服务器添加分片时，我的凭据不起作用

请勿使用“用户 ID=username@servername”形式的凭据，而只需使用“用户 ID = username”。  此外，请确保“用户名”登录名对分片具有权限。

## <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>是否我每次启动应用程序时，都需要创建分片映射管理器并填充分片

否 - 分片映射管理器（例如，[ShardMapManagerFactory.CreateSqlShardMapManager](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager)）只需创建一次。  在启动应用程序时，应用程序应使用调用 [ShardMapManagerFactory.TryGetSqlShardMapManager()](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager)。  每个应用程序域应该只有一个此类调用。

## <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>我在使用弹性数据库工具方面存在疑问，如何才能获得解答

请在 [Azure SQL 数据库论坛](https://social.msdn.microsoft.com/Forums/zh-cn/home?forum=ssdsgetstarted)上联系我们。

## <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>当使用分片键建立数据库连接的同时，仍然可以对同一分片上的其他分片键查询数据。  这是设计使然吗

弹性缩放 API 使用户能够连接到分片键的正确数据库，但不提供分片键筛选。  如果需要，请在查询中添加 **WHERE** 子句，以将范围限制到提供的分片键。

## <a name="can-i-use-a-different-sql-database-edition-for-each-shard-in-my-shard-set"></a>是否可对分片集中的每个分片使用不同的 SQL 数据库版本？

是的，每个分片是单独的数据库，因此，一个分片可以是高级版，而另一个可以是标准版。 此外，在分片的生命周期内，该分片的版本可以进行多次上调或下调。

## <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>在拆分或合并操作期间，“拆分/合并”工具是否会设置（或删除）数据库

否。 对于 **拆分** 操作，必须存在目标数据库和相应的架构，并且必须注册到分片映射管理器。  对于 **合并** 操作，必须从分片映射管理器中删除分片，并删除数据库。

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
