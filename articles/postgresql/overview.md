---
title: Azure Database for PostgreSQL 关系数据库服务概述
description: 概述了用于 PostgreSQL 关系数据库服务的 Azure 数据库。
author: WenJason
ms.author: v-jay
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
origin.date: 05/06/2019
ms.date: 11/04/2019
ms.openlocfilehash: 50ed14c038d901813f46a1ca1ff60527f9f00667
ms.sourcegitcommit: f643ddf75a3178c37428b75be147c9383384a816
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "73191531"
---
# <a name="what-is-azure-database-for-postgresql"></a>什么是用于 PostgreSQL 的 Azure 数据库？
Azure Database for PostgreSQL 是 Azure 云中为开发人员构建的关系型数据库服务。 它基于开源 [PostgreSQL](https://www.postgresql.org/) 数据库引擎的社区版本。

## <a name="azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL - 单一服务器
单一服务器部署选项提供：

- 没有额外费用的内置高可用性 (99.99% SLA)
- 使用非独占预付费定价，实现可预测性能
- 根据需要在数秒内垂直缩放
- 通过监视和警报功能快速评估缩放的影响
- 保护静态和动态敏感数据的安全
- 长达 35 天的自动备份和时间点还原
- 企业级安全性和符合性

所有这些功能几乎都不需要进行任何管理，并且都是在不另外收费的情况下提供的。 借助这些功能，用户可将注意力集中在如何快速进行应用程序开发、加快推向市场，而不需要花费宝贵的时间和资源来管理虚拟机与基础结构。 可以使用所选的开源工具和平台继续开发应用程序，不需学习新技能。

“单一服务器”部署选项提供三个定价层：“基本”、“常规用途”和“内存优化”。 每个层提供不同的资源功能以支持数据库工作负荷。 可以在一个月内花费很少的费用基于小型数据库构建第一个应用，然后根据解决方案的需求调整缩放。 动态可伸缩性使得数据库能够以透明方式对不断变化的资源需求做出响应。 只需在需要资源时为所需的资源付费。 有关详细信息，请参阅 [定价层](concepts-pricing-tiers.md)。

## <a name="data-security"></a>数据安全性
Azure Database for PostgreSQL 沿袭了 Azure 数据库服务的数据安全传统。 它的功能可以限制访问、保护静态数据和移动数据，以及帮助你监视活动。 有关 Azure 平台安全性的信息，请访问 [Azure 信任中心](https://www.trustcenter.cn/)。

Azure Database for PostgreSQL 服务使用 FIPS 140-2 验证的加密模块对静态数据进行存储加密。 数据（包括备份）在磁盘上加密，运行查询时创建的临时文件除外。 该服务使用包含在 Azure 存储加密中的 AES 256 位密码，并且密钥由系统进行管理。 存储加密始终处于启用状态，无法禁用。 默认情况下，Azure Database for PostgreSQL 服务要求对整个网络的动态数据以及数据库和客户端应用程序之间的动态数据实施安全连接。

## <a name="contacts"></a>联系人
若要联系 Azure 支持部门或修复帐户问题，请[通过 Azure 门户提交票证](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)。

## <a name="next-steps"></a>后续步骤
- 有关成本比较和计算器，请参阅[定价页](https://azure.cn/pricing/details/postgresql/)。
- 请从创建第一个 Azure Database for PostgreSQL [单一服务器](./quickstart-create-server-database-portal.md)开始
- 使用 Python、PHP、Ruby、C\#、Java、Node.js 构建第一个应用：[连接库](./concepts-connection-libraries.md)
