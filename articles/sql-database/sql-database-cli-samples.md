---
title: 适用于 SQL 数据库的 Azure CLI 脚本示例 | Microsoft Docs
description: 创建和管理 Azure SQL 数据库服务器、弹性池、数据库和防火墙的 Azure CLI 脚本示例。
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples, mvc
ms.devlang: azurecli
ms.topic: sample
author: WenJason
ms.author: v-jay
ms.reviewer: ''
manager: digimobile
origin.date: 02/03/2019
ms.date: 03/25/2019
ms.openlocfilehash: 61ffeca3d33027097bacd1d37166e037a380ba61
ms.sourcegitcommit: 02c8419aea45ad075325f67ccc1ad0698a4878f4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318879"
---
# <a name="azure-cli-samples-for-azure-sql-database"></a>适用于 Azure SQL 数据库的 Azure CLI 示例

可以使用 <a href="/cli/azure">Azure CLI</a> 配置 Azure SQL 数据库。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

可以在本地安装并使用 CLI，本主题要求运行 Azure CLI 2.0 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI]( /cli/install-azure-cli)。 

## <a name="single-database--elastic-pools"></a>单一数据库和弹性池

下表包括适用于 Azure SQL 数据库的 Azure CLI 脚本示例的链接。

| |  |
|---|---|
|**创建单一数据库和弹性池**||
| [创建单一数据库和配置防火墙规则](scripts/sql-database-create-and-configure-database-cli.md) | 此 CLI 脚本示例创建单个 Azure SQL 数据库并配置服务器级防火墙规则。 |
| [创建弹性池并移动入池数据库](scripts/sql-database-move-database-between-pools-cli.md) | 此 CLI 脚本示例创建 SQL 弹性池，移动入池 Azure SQL 数据库，并更改计算大小。|
|**缩放单一数据库和弹性池**||
| [缩放单一数据库](scripts/sql-database-monitor-and-scale-database-cli.md) | 此 CLI 脚本示例在查询数据库的大小信息后，将单个 Azure SQL 数据库缩放为不同的计算大小。 |
| [缩放弹性池](scripts/sql-database-scale-pool-cli.md) | 此 CLI 脚本示例将 SQL 弹性池缩放为不同的计算大小。  |
|||

详细了解[单一数据库 Azure CLI API](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases)。

