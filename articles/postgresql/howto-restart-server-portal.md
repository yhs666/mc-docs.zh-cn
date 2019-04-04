---
title: 使用 Azure 门户重启 Azure Database for PostgreSQL 服务器
description: 本文介绍如何使用 Azure 门户重启 Azure Database for PostgreSQL 服务器。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 3/18/2019
ms.date: 04/01/2019
ms.openlocfilehash: 6452bddc5179068b66bfd7c1a4ced6a82ad0d97d
ms.sourcegitcommit: 5b827b325a85e1c52b5819734ac890d2ed6fc273
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2019
ms.locfileid: "58503561"
---
# <a name="restart-azure-database-for-postgresql-server-using-the-azure-portal"></a>使用 Azure 门户重启 Azure Database for PostgreSQL 服务器
本主题介绍如何重启 Azure Database for PostgreSQL 服务器。 出于维护原因，可能需要重启服务器，这会在服务器执行操作时导致短暂中断。

如果服务处于繁忙状态，则会阻止重启服务器。 例如，服务可能在处理缩放 vCore 等先前请求的操作。
 
完成重启所需的时间取决于 PostgreSQL 恢复过程。 若要减少重启时间，建议在重启之前尽量减少服务器上发生的活动量。

## <a name="prerequisites"></a>先决条件
若要完成本操作指南，需要：
- [Azure Database for PostgreSQL 服务器和数据库](quickstart-create-server-database-portal.md)

## <a name="perform-server-restart"></a>执行服务器重启

可通过以下步骤重启 PostgreSQL 服务器：

1. 在 [Azure 门户](https://portal.azure.cn/)中，选择 Azure Database for PostgreSQL 服务器。

2. 在服务器“概述”页的工具栏中，单击“重启”。

   ![Azure Database for PostgreSQL - 概览 -“重启”按钮](./media/howto-restart-server-portal/2-server.png)

3. 单击“是”以确认重启服务器。

   ![Azure Database for PostgreSQL - 重启确认](./media/howto-restart-server-portal/3-restart-confirm.png)

4. 观察到服务器状态更改为“正在重启”。

   ![Azure Database for PostgreSQL - 重启状态](./media/howto-restart-server-portal/4-restarting-status.png)

5. 确认服务器重启成功。

   ![Azure Database for PostgreSQL - 重启成功](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>后续步骤

了解[如何在 Azure Database for PostgreSQL 中设置参数](howto-configure-server-parameters-using-portal.md)