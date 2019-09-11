---
title: Azure SQL 数据库托管实例自定义 DNS | Microsoft Docs
description: 本主题介绍使用 Azure SQL 数据库托管实例的自定义 DNS 的配置选项。
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: sstein, bonova, carlrab
origin.date: 07/17/2019
ms.date: 09/09/2019
ms.openlocfilehash: b15ea1c5b23fe1fbe5cd4646db599fa735f9d068
ms.sourcegitcommit: 2610641d9fccebfa3ebfffa913027ac3afa7742b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2019
ms.locfileid: "70372973"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>为 Azure SQL 数据库托管实例配置自定义 DNS

Azure SQL 数据库托管实例必须在 Azure [虚拟网络 (VNet)](../virtual-network/virtual-networks-overview.md) 中部署。 有几个方案（例如，数据库邮件，将服务器链接到云或混合环境中的其他 SQL 实例）需要从托管实例解析专用主机名。 在这种情况下，需要在 Azure 中配置自定义 DNS。 

由于托管实例对内部工作使用同一 DNS，你需要对自定义 DNS 服务器进行配置，使之能够解析公共域名。

   > [!IMPORTANT]
   > 始终对邮件服务器、SQL Server 和其他服务使用完全限定的域名 (FQDN)，即使它们位于专用 DNS 区域内也是如此。 例如，对邮件服务器使用 `smtp.contoso.com`，因为无法正确解析简单的 `smtp`。

   > [!IMPORTANT]
   > 更新虚拟网络 DNS 服务器不会立即影响托管实例。 托管实例 DNS 配置会在 DHCP 租约过期后或平台升级后进行更新，具体取决于哪一项先发生。 **建议用户在创建第一个托管实例之前，先设置其虚拟网络 DNS 配置。**

## <a name="next-steps"></a>后续步骤

- 有关概述，请参阅[什么是托管实例](sql-database-managed-instance.md)
- 有关演示如何新建托管实例的教程，请参阅[创建托管实例](sql-database-managed-instance-get-started.md)。
- 有关为托管实例配置 VNet 的信息，请参阅[托管实例的 VNet 配置](sql-database-managed-instance-connectivity-architecture.md)
