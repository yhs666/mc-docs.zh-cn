---
title: 在 Azure Database for MySQL 中创建和管理只读副本
description: 本文介绍如何使用门户在 Azure Database for MySQL 中设置和管理只读副本。
services: mysql
author: WenJason
ms.author: v-jay
editor: jasonwhowell
ms.service: mysql
ms.topic: conceptual
origin.date: 09/24/2018
ms.date: 12/03/2018
ms.openlocfilehash: 8f740f2dc54e3002e6e9867b8a76b9afa4bd09ac
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52673252"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-portal"></a>如何使用 Azure 门户在 Azure Database for MySQL 中创建和管理只读副本

> [!NOTE] 
> 将要查看的是 Azure Database for MySQL 的新服务。 若要查看经典 MySQL Database for Azure 的文档，请访问[此页](https://docs.azure.cn/zh-cn/mysql-database-on-azure/)。

在本文中，将了解如何使用 Azure 门户在与 Azure Database for MySQL 服务中的主服务器相同的 Azure 区域内创建和管理只读副本。 此功能目前处于公开预览状态。

## <a name="prerequisites"></a>先决条件

- 将用作主服务器的 [Azure Database for MySQL 服务器](quickstart-create-mysql-server-database-using-azure-portal.md)。

> [!IMPORTANT]
> 只读副本功能仅适用于“常规用途”或“内存优化”定价层中的 Azure Database for MySQL 服务器。 请确保主服务器位于其中一个定价层中。

## <a name="create-a-read-replica"></a>创建只读副本

可以使用以下步骤创建只读副本服务器：

1. 登录到 [Azure 门户](https://portal.azure.cn/)。

2. 选择要用作主服务器的现有 Azure Database for MySQL 服务器。 此操作将打开“概述”页。

3. 从菜单中的“设置”下，选择“复制”。

4. 选择“添加副本”。

   ![Azure Database for MySQL - 复制 ](./media/howto-read-replica-portal/add-replica.png)

5. 输入副本服务器的名称，然后单击“确定”以确认创建副本。

   ![Azure Database for MySQL - 创建副本 ](./media/howto-read-replica-portal/create-replica.png)

> [!NOTE]
> 只读副本使用与主服务器相同的服务器配置创建。 副本服务器配置在创建后可以更改。 建议副本服务器的配置应保持在与主服务器相同或更大的值，以确保副本能够跟上主服务器。

一旦创建副本服务器，可以从“复制”边栏选项卡中进行查看。

   ![Azure Database for MySQL - 列出副本 ](./media/howto-read-replica-portal/list-replica.png)

## <a name="stop-replication-to-a-replica-server"></a>停止复制到副本服务器

> [!IMPORTANT]
> 停止复制到服务器操作不可逆。 一旦主服务器和副本服务器之间的复制停止，无法撤消。 然后，副本服务器将成为独立服务器，并且现在支持读取和写入。 此服务器不能再次成为副本服务器。

若要从 Azure 门户停止主服务器和副本服务器之间的复制，请使用以下步骤：

1. 在 Azure 门户中，选择主 Azure Database for MySQL 服务器。 

2. 从菜单中的“设置”下，选择“复制”。

3. 选择要停止复制的副本服务器。

   ![Azure Database for MySQL - 停止复制选择服务器 ](./media/howto-read-replica-portal/stop-replication-select.png)

4. 选择“停止复制”。

   ![Azure Database for MySQL - 停止复制 ](./media/howto-read-replica-portal/stop-replication.png)

5. 通过单击“确定”，确认要停止复制。

   ![Azure Database for MySQL - 停止复制确认 ](./media/howto-read-replica-portal/stop-replication-confirm.png)

## <a name="delete-a-replica-server"></a>删除副本服务器

若要从 Azure 门户删除只读副本服务器，请使用以下步骤：

1. 在 Azure 门户中，选择主 Azure Database for MySQL 服务器。

2. 从菜单中的“设置”下，选择“复制”。

3. 选择要删除的副本服务器。

   ![Azure Database for MySQL - 删除副本选择服务器 ](./media/howto-read-replica-portal/delete-replica-select.png)

4. 选择“删除副本”

   ![Azure Database for MySQL - 删除副本 ](./media/howto-read-replica-portal/delete-replica.png)

5. 键入副本的名称，然后单击“删除”以确认删除副本。  

   ![Azure Database for MySQL - 删除副本确认 ](./media/howto-read-replica-portal/delete-replica-confirm.png)

## <a name="delete-a-master-server"></a>删除主服务器

> [!IMPORTANT]
> 删除主服务器会停止复制到所有副本服务器，并删除主服务器本身。 副本服务器成为现在支持读取和写入的独立服务器。

若要从 Azure 门户删除主服务器，请使用以下步骤：

1. 在 Azure 门户中，选择主 Azure Database for MySQL 服务器。

2. 从“概览”中，选择“删除”。

   ![Azure Database for MySQL - 删除主服务器 ](./media/howto-read-replica-portal/delete-master-overview.png)

3. 键入主服务器的名称，然后单击“删除”以确认删除主服务器。  

   ![Azure Database for MySQL - 删除主服务器 ](./media/howto-read-replica-portal/delete-master-confirm.png)

## <a name="next-steps"></a>后续步骤

- 详细了解[只读副本](concepts-read-replicas.md)