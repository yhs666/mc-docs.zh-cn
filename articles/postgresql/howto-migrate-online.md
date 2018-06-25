---
title: 在最短的停机时间内迁移到 Azure Database for PostgreSQL
description: 本文介绍如何使用 Azure 数据库迁移服务，在尽量减少停机时间的情况下将 PostgreSQL 数据库迁移到 Azure Database for PostgreSQL。
services: postgresql
author: v-chenyh
ms.author: v-chenyh
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/22/2018
ms.openlocfilehash: 6ff063dd42c0b7c0ce1127c6b2baecbbc03fe921
ms.sourcegitcommit: d744d18624d2188adbbf983e1c1ac1110d53275c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314364"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>在最短的停机时间内迁移到 Azure Database for PostgreSQL
可以使用新引入的适用于 [Azure 数据库迁移服务](https://aka.ms/get-dms) (DMS) 的**连续同步功能**，在尽量减少停机时间的情况下将 PostgreSQL 迁移到 Azure Database for PostgreSQL。 此功能限制应用程序引发的停机时间。

## <a name="overview"></a>概述
DMS 会通过初始加载将本地数据加载到 Azure Database for PostgreSQL，然后在应用程序保持运行的情况下，持续将任何新的事务同步到 Azure。 当数据在目标 Azure 端赶上进度以后，即可停止应用程序一段时间（最短停机时间），等待目标端的最后一批数据（从停止应用程序开始计算，直至应用程序事实上不可用，以便获取新的流量）赶上，然后更新连接字符串，使之指向 Azure。 完成以后，应用程序将留存在 Azure 中！

![使用 Azure 数据库迁移服务进行连续同步](./media/howto-migrate-online/ContinuousSync.png)

通过 DMS 来迁移 PostgreSQL 源的功能目前为预览版。 若要试用此服务来迁移 PostgreSQL 工作负荷，请通过 Azure DMS [预览页](https://aka.ms/dms-preview)进行注册，表达你的意愿。 你的反馈对于我们改进服务很有用。

## <a name="next-steps"></a>后续步骤
- 观看视频：[App Modernization with Microsoft Azure](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102)（使用 Microsoft Azure 实现应用现代化），了解如何将 PostgreSQL 应用迁移到 Azure Database for PostgreSQL。
- 通过 Azure DMS [预览页](https://aka.ms/dms-preview)注册有限预览版，在尽量减少停机时间的情况下将 PostgreSQL 迁移到 Azure Database for PostgreSQL。
