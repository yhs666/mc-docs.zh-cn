---
title: Azure Database for MariaDB 中支持的版本
description: 说明 Azure Database for MariaDB 中支持的版本。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.topic: conceptual
origin.date: 09/24/2018
ms.date: 05/27/2019
ms.openlocfilehash: 2e37d76da324411dac0f0f1384f5ceb93ba44b1d
ms.sourcegitcommit: 60169f39663ae62016f918bdfa223c411e249883
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2019
ms.locfileid: "66173289"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>支持的 Azure Database for MariaDB 服务器版本
已使用 InnoDB 引擎从开放源代码 [MariaDB 服务器](https://downloads.mariadb.org/)开发了 Azure Database for MariaDB。 Azure Database for MariaDB 目前支持以下版本：

## <a name="mariadb-version-10217"></a>MariaDB 版本 10.2.17
若要详细了解 MariaDB 10.2.17 中的改进和修复，请参阅 [MariaDB 文档](https://downloads.mariadb.org/mariadb/10.2.17/)。

> [!NOTE]
> 在服务中，网关用于将连接重定向到服务器实例。 建立连接后，MySQL 客户端显示网关中设置的 MariaDB 版本，而不是 MariaDB 服务器实例上运行的实际版本。 若要确定 MariaDB 服务器实例的版本，可在 MySQL 提示符处使用 `SELECT VERSION();` 命令。

## <a name="managing-updates-and-upgrades"></a>管理更新和升级
该服务会自动管理针对次要版本更新的修补。

## <a name="next-steps"></a>后续步骤
有关基于服务层级的具体资源配额和限制的信息，请参阅[服务层级](./concepts-pricing-tiers.md)
