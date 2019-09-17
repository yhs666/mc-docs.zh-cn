---
title: 在最短的停机时间内迁移到 Azure Database for PostgreSQL（单一服务器）
description: 本文介绍如何使用 Azure 数据库迁移服务，在最短的停机时间内将 PostgreSQL 数据库迁移到 Azure Database for PostgreSQL（单一服务器）。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 5/6/2019
ms.date: 09/02/2019
ms.openlocfilehash: a3a0d7d87fd78dd1ebc18cb5df47b56a5edd7516
ms.sourcegitcommit: 3f0c63a02fa72fd5610d34b48a92e280c2cbd24a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2019
ms.locfileid: "70132756"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql---single-server"></a>在最短的停机时间内迁移到 Azure Database for PostgreSQL（单一服务器）
可以使用新引入的适用于 [Azure 数据库迁移服务](/dms/) (DMS) 的**连续同步功能**，在尽量减少停机时间的情况下将 PostgreSQL 迁移到 Azure Database for PostgreSQL。 此功能限制应用程序引发的停机时间。

## <a name="overview"></a>概述
Azure DMS 将本地内容初始加载到 Azure Database for PostgreSQL，然后在应用程序仍在运行时不断地将任何新事务同步到 Azure。 当数据在目标 Azure 端赶上进度以后，即可停止应用程序一段时间（最短停机时间），等待目标端的最后一批数据（从停止应用程序开始计算，直至应用程序事实上不可用，以便获取新的流量）赶上，然后更新连接字符串，使之指向 Azure。 完成以后，应用程序将留存在 Azure 中！

![使用 Azure 数据库迁移服务进行连续同步](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>后续步骤
- 请参阅教程[使用 DMS 以联机方式将 PostgreSQL 迁移到 Azure Database for PostgreSQL](/dms/tutorial-postgresql-azure-postgresql-online)。
