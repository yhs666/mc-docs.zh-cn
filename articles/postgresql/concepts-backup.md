---
title: 在 Azure Database for PostgreSQL 中进行备份和还原
description: 了解如何自动备份和还原 Azure Database for PostgreSQL 服务器。
services: postgresql
author: v-chenyh
ms.author: v-chenyh
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: fd4bc35ab79d9870a89ee17734d96ac910a0e251
ms.sourcegitcommit: d744d18624d2188adbbf983e1c1ac1110d53275c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314374"
---
# <a name="backup-and-restore-in-azure-database-for-postgresql"></a>在 Azure Database for PostgreSQL 中进行备份和还原

Azure Database for PostgreSQL 可自动创建服务器备份并将其存储在用户配置的本地冗余或异地冗余存储中。 备份可以用来将服务器还原到某个时间点。 备份和还原是任何业务连续性策略的基本组成部分，因为它们可以保护数据免遭意外损坏或删除。

## <a name="backups"></a>备份

Azure Database for PostgreSQL 可以进行完整备份、差异备份和事务日志备份。 可以通过这些备份将服务器还原到所配置的备份保留期中的任意时间点。 默认的备份保留期为七天。 可以选择将其配置为长达 35 天。 所有备份都使用 AES 256 位加密进行加密。

### <a name="backup-frequency"></a>备份频率

通常情况下，完整备份每周进行一次，差异备份每天进行两次，事务日志备份每五分钟进行一次。 第一次完整备份在创建服务器后立即进行计划。 初始备份在大型已还原服务器上可能耗时较长。 新服务器可以还原到的最早时间点是完成初始完整备份的时间。

### <a name="backup-storage-cost"></a>备份存储成本

Azure Database for PostgreSQL 最高可以提供 100% 的已预配服务器存储作为备份存储，不收取任何额外费用。 通常情况下，这适合将备份保留七天。 超出的备份存储使用量按每月每 GB 标准收费。

例如，如果为服务器预配了 250 GB，则有 250 GB 不额外收费的备份存储。 存储超出 250 GB 就会收费。

## <a name="restore"></a>还原

在 Azure Database for PostgreSQL 中进行还原时，会根据原始服务器的备份创建新的服务器。

- **时间点还原**：可以与任一备份冗余选项配合使用，所创建的新服务器与原始服务器位于同一区域。

估计的恢复时间取决于若干因素，包括数据库大小、事务日志大小、网络带宽，以及在同一区域同时进行恢复的数据库总数。 恢复时间通常少于 12 小时。

> [!IMPORTANT]
> 如果删除服务器，则属于该服务器的所有数据库也会被删除且不可恢复。 无法还原已删除的服务器。

### <a name="point-in-time-restore"></a>时间点还原

可以还原到备份保留期中的任意时间点。 新服务器在原始服务器所在的 Azure 区域中创建。 它在创建时，使用原始服务器在定价层、计算的代、vCore 数、存储大小、备份保留期和备份冗余选项方面的配置。

多种情况下可以使用时间点还原。 例如，用户意外删除了数据、删除了重要的表或数据库，或者应用程序因为缺陷而意外地使用错误数据覆盖了正确数据。

可能需要等到下一个事务日志备份进行后，才能还原到上一个五分钟内的某个时间点。

### <a name="perform-post-restore-tasks"></a>执行还原后任务

从任一恢复机制还原后，都应执行以下任务，然后用户和应用程序才能重新运行：

- 如果需要使用新服务器来替换原始服务器，则请将客户端和客户端应用程序重定向到新服务器
- 对于要进行连接的用户，请确保设置适当的服务器级防火墙规则
- 确保设置适当的登录名和数据库级权限
- 视情况配置警报

## <a name="next-steps"></a>后续步骤

- 若要详细了解业务连续性，请参阅 [业务连续性概述](concepts-business-continuity.md)。
- 若要使用 Azure 门户还原到某个时间点，请参阅 [使用 Azure 门户将数据库还原到某个时间点](howto-restore-server-portal.md)。
- 若要使用 Azure CLI 还原到某个时间点，请参阅 [使用 CLI 将数据库还原到某个时间点](howto-restore-server-cli.md)。