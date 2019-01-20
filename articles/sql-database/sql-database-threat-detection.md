---
title: 威胁检测 - Azure SQL 数据库 | Microsoft 文档
description: 威胁检测会检测异常的数据库活动，指出存在对单一数据库或弹性池中数据库的潜在安全威胁。
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto, carlrab
manager: digimobile
origin.date: 01/11/2019
ms.date: 01/21/2019
ms.openlocfilehash: 1d0f0d30ca2d9ae6fef44d9c2a6b0f13a75b3576
ms.sourcegitcommit: 2edae7e4dca37125cceaed89e0c6e4502445acd0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2019
ms.locfileid: "54363769"
---
# <a name="azure-sql-database-threat-detection-for-single-database"></a>针对单一数据库的 Azure SQL 数据库威胁检测

针对 [SQL 数据库](sql-database-technical-overview.md)单一数据库的 Azure SQL [威胁检测](sql-database-threat-detection-overview.md)可以检测异常活动，这些活动指示数据库遭到了异常以及可能对数据库有害的访问或利用尝试。 威胁检测可以识别“潜在的 SQL 注入”、“来自异常位置或数据中心的访问”、“来自陌生主体或可能有害的应用程序的访问”以及“暴力攻击 SQL 凭据”- 请在[威胁检测警报](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts)中查看详细信息。

可以通过 [Azure 门户](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal)接收有关检测到的威胁的通知

[威胁检测](sql-database-threat-detection-overview.md)是 [SQL 高级威胁防护](sql-advanced-threat-protection.md) (ATP) 产品/服务（它是高级 SQL 安全功能的一个统一包）的一部分。 可通过中心 SQL ATP 门户访问和管理威胁检测。

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>在 Azure 门户中为数据库设置威胁检测

1. 在 [https://portal.azure.cn](https://portal.azure.cn) 中启动 Azure 门户。
2. 导航到要保护的 Azure SQL 数据库服务器的配置页。 在安全设置中，选择“高级威胁防护”。
3. 在“高级威胁防护”配置页上：

   - 在服务器上启用高级威胁防护。
   - 在“威胁检测设置”中的“发送警报到”文本框中，提供检测到异常数据库活动时收到安全警报的电子邮件列表。
  
   ![设置威胁检测](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>使用 PowerShell 设置威胁检测

有关脚本示例，请参阅[使用 PowerShell 配置审核和威胁检测](scripts/sql-database-auditing-and-threat-detection-powershell.md)。

## <a name="next-steps"></a>后续步骤

* 详细了解[威胁检测](sql-database-threat-detection-overview.md)。
* 详细了解 [SQL 高级威胁防护](sql-advanced-threat-protection.md)。 
* 了解有关 [Azure SQL 数据库审核](sql-database-auditing.md)的详细信息
* 有关定价的更多详细信息，请参阅 [SQL 数据库定价页](https://azure.cn/pricing/details/sql-database/)  
