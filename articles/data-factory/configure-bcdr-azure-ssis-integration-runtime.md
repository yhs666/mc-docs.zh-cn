---
title: 为 SQL 数据库故障转移配置 Azure-SSIS Integration Runtime | Microsoft Docs
description: 本文介绍了如何针对 Azure SQL 数据库异地复制和 SSISDB 数据库的故障转移配置 Azure-SSIS Integration Runtime。
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
origin.date: 08/14/2018
ms.date: 07/08/2019
author: WenJason
ms.author: v-jay
ms.reviewer: douglasl
manager: digimobile
ms.openlocfilehash: 1d02101d8253c32429c8c16b855dbcf775f438f6
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67569869"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-azure-sql-database-geo-replication-and-failover"></a>针对 Azure SQL 数据库异地复制和故障转移配置 Azure-SSIS Integration Runtime

本文介绍了如何针对 Azure SQL 数据库异地复制和 SSISDB 数据库配置 Azure-SSIS Integration Runtime。 发生故障转移时，你可以确保 Azure-SSIS IR 使用辅助数据库保持工作。

有关 SQL 数据库的异地复制和故障转移的详细信息，请参阅[概述：活动异地复制和自动故障转移组](../sql-database/sql-database-geo-replication-overview.md)。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>方案 - Azure-SSIS IR 指向读写侦听器终结点

### <a name="conditions"></a>Conditions

当以下条件成立时本部分内容适用：

- Azure-SSIS IR 指向故障转移组的读写侦听器终结点。

  AND

- SQL 数据库服务器“未”  配置虚拟网络服务终结点规则。

### <a name="solution"></a>解决方案

发生故障转移时，它对 Azure-SSIS IR 是透明的。 Azure-SSIS IR 会自动连接到故障转移组的新的主数据库。

## <a name="next-steps"></a>后续步骤

考虑 Azure-SSIS IR 的以下其他配置选项：

- [配置 Azure-SSIS 集成运行时以实现高性能](configure-azure-ssis-integration-runtime-performance.md)

- [自定义 Azure-SSIS 集成运行时的设置](how-to-configure-azure-ssis-ir-custom-setup.md)

- [预配 Azure-SSIS 集成运行时企业版](how-to-configure-azure-ssis-ir-enterprise-edition.md)
