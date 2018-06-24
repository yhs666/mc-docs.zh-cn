---
title: 如何在 Azure Database for PostgreSQL 中还原服务器
description: 本文介绍如何使用 Azure 门户在 Azure Database for PostgreSQL 中还原服务器。
services: postgresql
author: v-chenyh
ms.author: v-chenyh
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/22/2018
ms.openlocfilehash: 31068ba4f4e629b26c69cbbd54cfda95acd2d6a5
ms.sourcegitcommit: d744d18624d2188adbbf983e1c1ac1110d53275c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314358"
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a>如何使用 Azure 门户在 Azure Database for PostgreSQL 中备份和还原服务器

## <a name="backup-happens-automatically"></a>自动进行备份
Azure Database for PostgreSQL 服务器定期进行备份以便启用还原功能。 通过此功能，用户可将服务器及其所有数据库还原到新服务器上的某个较早时间点。

## <a name="set-backup-configuration"></a>设置备份配置

可以通过以下步骤更改服务器的备份保留期：
1. 登录到 [Azure 门户](https://portal.azure.cn/)。
2. 选择你的 Azure Database for PostgreSQL 服务器。 此操作将打开“概述”页。
3. 在“设置”下，从菜单中选择“定价层”。 使用滑块可以根据需要更改**备份保留期**（7 天到 35 天）。
在下面的屏幕截图中，该项已增加到 34 天。
![增加的备份保留期](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. 单击“确定”确认更改。

备份保留期控制可以往回检索多长时间的时间点还原，因为它基于可用备份。 以下部分进一步说明了时间点还原。 

## <a name="point-in-time-restore"></a>时间点还原
使用 Azure Database for PostgreSQL 可以将服务器还原到某个时间点，并还原到服务器的新副本。 可以使用此新服务器恢复数据，或将客户端应用程序指向此新服务器。

例如，如果今天中午不小心删除了一张表，则可以将该表还原到中午之前的状态，并从服务器的新副本中检索丢失的表和数据。 时间点还原在服务器级别（而不是在数据库级别）进行。

以下步骤演示将示例服务器还原到某个时间点：
1. 在 Azure 门户中，选择 Azure Database for PostgreSQL 服务器。 

2. 在服务器“概述”页的工具栏中，选择“还原”。

   ![Azure Database for PostgreSQL - 概述 - 还原按钮](./media/howto-restore-server-portal/2-server.png)

3. 使用必需信息填写“还原”窗体：

   ![Azure Database for PostgreSQL - 还原信息 ](./media/howto-restore-server-portal/3-restore.png)
  - **还原点**：选择要还原到的时间点。
  - **目标服务器**：提供新服务器的名称。
  - 位置：不可选择区域。 默认情况下，此值与源服务器相同。
  - **定价层**：执行时间点还原时，无法更改这些参数。 此值与源服务器相同。 

4. 单击“确定”，将服务器还原到某个时间点。 

5. 还原完成后，找到创建的新服务器，以验证数据是否已按预期还原。

>[!Note]
>通过时间点还原创建的新服务器具有在所选时间点对现有服务器有效的相同服务器管理员登录名和密码。 可以从新服务器的“概述”页更改密码。

## <a name="next-steps"></a>后续步骤
- 详细了解服务的[备份](concepts-backup.md)。
- 详细了解[业务连续性](concepts-business-continuity.md)选项。
