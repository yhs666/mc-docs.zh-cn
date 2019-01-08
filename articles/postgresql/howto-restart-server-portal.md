---
title: 使用 Azure 门户重启 Azure Database for PostgreSQL 服务器
description: 本文介绍如何使用 Azure 门户重启 Azure Database for PostgreSQL 服务器。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 11/16/2018
ms.date: 12/31/2018
ms.openlocfilehash: 01850f8090242be70a97e180c32c2680709b1037
ms.sourcegitcommit: e96e0c91b8c3c5737243f986519104041424ddd5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806130"
---
# <a name="restart-azure-database-for-postgresql-server-using-azure-portal"></a>使用 Azure 门户重启 Azure Database for PostgreSQL 服务器
本主题介绍如何重启 Azure Database for PostgreSQL 服务器。 出于维护原因，可能需要重启服务器，这会在服务器执行操作时导致短暂中断。

如果服务处于繁忙状态，则会阻止重启服务器。 例如，服务可能在处理缩放 vCore 等先前请求的操作。
 
完成重启所需的时间取决于 PostgreSQL 恢复过程。 若要减少重启时间，建议在重启之前尽量减少服务器上发生的活动量。

## <a name="prerequisites"></a>先决条件
若要完成本操作指南，需要：
- [Azure Database for PostgreSQL 服务器和数据库](quickstart-create-server-database-portal.md)

## <a name="perform-server-restart"></a>执行服务器重启

可通过以下步骤重启 PostgreSQL 服务器：

1. 在 Azure 门户中，选择 Azure Database for PostgreSQL 服务器。

2. 在服务器“概述”页的工具栏中，单击“重启”。

   ![Azure Database for PostgreSQL - 概览 -“重启”按钮](./media/howto-restart-server-portal/2-server.png)

3. 单击“是”以确认重启服务器。

   ![Azure Database for PostgreSQL - 重启确认 ](./media/howto-restart-server-portal/3-restart-confirm.png)

4. 观察到服务器状态更改为“正在重启”。

   ![Azure Database for PostgreSQL - 重启状态 ](./media/howto-restart-server-portal/4-restarting-status.png)

5. 确认服务器重启成功。

   ![Azure Database for PostgreSQL - 重启成功 ](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>后续步骤

[快速入门：使用 Azure 门户创建 Azure Database for PostgreSQL 服务器](./quickstart-create-server-database-portal.md)