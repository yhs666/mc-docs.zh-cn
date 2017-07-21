---
title: "服务器上的数据库当前不可用，请连接到 SQL 数据库 | Azure"
description: "对应用程序连接到 SQL 数据库时发生的“服务器上的数据库当前不可用”错误进行排查。"
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
keywords: "服务器上的数据库当前不可用, 连接到 SQL 数据库"
ms.assetid: f61999ac-d46b-448a-8830-3b04978d84ec
ms.service: sql-database
ms.custom: troubleshoot
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/20/2017
ms.date: 03/24/2017
ms.author: v-johch
ms.openlocfilehash: b1885c27aa02813b5c9488b5f2de8c5a8218a2c6
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>连接到 SQL 数据库时发生“服务器上的数据库当前不可用”错误
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

当应用程序连接到 Azure SQL 数据库时，你将收到以下错误消息：

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [!NOTE]
> 此错误消息通常是暂时的（生存期较短）。

移动（或重新配置）Azure 数据库时发生此错误，应用程序失去与 SQL 数据库的连接。 计划内事件（例如软件升级）或计划外事件（例如进程崩溃或负载均衡）都可能导致发生 SQL 数据库重新配置事件。 大多数重新配置事件的生存期通常较短，应在最多 60 秒内完成。 但是，这些事件偶尔可能需要更长时间才能完成，例如当大型事务导致长时间运行的恢复时。

## <a name="steps-to-resolve-transient-connectivity-issues"></a>解决暂时性连接问题的步骤
1. 检查 [Azure 服务仪表板](https://www.azure.cn/support/service-dashboard/)在由应用程序报告错误期间是否发生任何已知的服务中断。
2. 连接到云服务的应用程序（如 Azure SQL 数据库）应期望定期重新配置事件并实施重试逻辑来处理这些错误，而不是将它们作为应用程序错误展现给用户。 查看[暂时性错误](./sql-database-connectivity-issues.md)部分和 [SQL 数据库开发概述](./sql-database-develop-overview.md)中的最佳实践和设计准则以了解更多信息和常规重试策略。 然后查看 [SQL 数据库和 SQL Server 的连接库](./sql-database-libraries.md)上的代码示例了解详细信息。
3. 由于数据库即将达到其资源限制，因此错误看起来像是暂时性连接问题。 请参阅[解决性能问题](./sql-database-troubleshoot-performance.md)。
4. 如果连接问题继续存在，或者应用程序发生错误的持续时间超过 60 秒或在特定的一天中看到错误多次发生，请通过在 [Azure 支持](https://www.azure.cn/support/contact)网站上选择“**获取支持**”提出 Azure 支持请求。

## <a name="next-steps"></a>后续步骤
- 如果收到不同的错误，请评估[错误消息](./sql-database-develop-error-messages.md)，提供错误原因的线索。
- 如果问题持续出现，请访问[排查 Azure SQL 数据库的常见连接问题](./sql-database-troubleshoot-common-connection-issues.md)中的指南。