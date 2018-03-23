---
title: Azure Stack 上的 SQL 宿主服务器 | Microsoft Docs
description: 如何添加 SQL 实例以通过 SQL 适配器资源提供程序进行预配
services: azure-stack
documentationCenter: ''
author: JeffGoldner
manager: bradleyb
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 12/14/2017
ms.date: 03/04/2018
ms.author: v-junlch
ms.openlocfilehash: 660ea9b7b6c7d1bcbaa80f07e7399cfe5143a0a7
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="add-hosting-servers-for-use-by-the-sql-adapter"></a>添加供 SQL 适配器使用的宿主服务器

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

可以使用 [Azure Stack](azure-stack-poc.md) 内部 VM 上的 SQL 实例，或者 Azure Stack 环境外部的实例，前提是资源提供程序能够连接到这些实例。 一般要求为：

- SQL 实例必须专用于 RP 和用户工作负荷。 不能使用其他任何用户（包括 应用服务）正在使用的 SQL 实例。
- RP 适配器未加入域，并只可以使用 SQL 身份验证进行连接。
- 必须配置具有相应特权、可供 RP 使用的帐户。
- RP 和用户（例如 Web 应用）使用用户网络，因此需要连接到此网络上的 SQL 实例。 此要求通常意味着 SQL 实例的 IP 必须在公共网络上。
- 你负责管理 SQL 实例及其主机；RP 不会执行修补、备份、凭据轮换等操作。
- 可以使用 SKU 创建各类 SQL 功能，例如性能、Always On 等等。


可通过 Marketplace 管理功能获取许多 SQL IaaS 虚拟机映像。 在使用 Marketplace 项部署 VM 之前，请确保下载最新版本的 SQL IaaS 扩展。 SQL 映像与 Azure 中提供的 SQL VM 相同。 对于从这些映像创建的 SQL VM，IaaS 扩展和相应的门户增强功能可提供自动修补和备份等功能。

可以使用其他选项部署 SQL VM，包括 [Azure Stack 快速入门库](https://github.com/Azure/AzureStack-QuickStart-Templates)中的模板。

> [!NOTE]
> 必须通过用户订阅创建安装在多节点 Azure Stack 上的任何宿主服务器， 而不能通过默认提供程序订阅创建。 必须通过用户门户或者使用相应的登录名通过 PowerShell 会话来创建这些服务器。 所有宿主服务器都是可计费的 VM，并且必须具有相应的 SQL 许可证。 服务管理员可以是订阅的所有者。


### <a name="required-privileges"></a>所需的特权

可以创建权限比完全 sysadmin 特权更低的新管理用户。 需要允许的特定操作为：

- 数据库：创建、更改、使用包含（仅限 Always On）、删除、备份
- 可用性组：更改、联接、添加/删除数据库
- 登录：创建、选择、更改、删除、吊销
- 选择操作：\[master\].\[sys\].\[availability_group_listeners\] (AlwaysOn)、sys.availability_replicas (AlwaysOn)、sys.databases、\[master\].\[sys\].\[dm_os_sys_memory\]、SERVERPROPERTY、\[master\].\[sys\].\[availability_groups\] (AlwaysOn)、sys.master_files



## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>通过连接到独立宿主 SQL 服务器来提供容量。
可以运行使用任何版本的 SQL Server 2014 或 SQL Server 2016 的独立（非 HA）SQL 服务器。 确保已准备好拥有管理特权的帐户的凭据。

若要添加已预配的独立宿主服务器，请遵循以下步骤：

1. 以服务管理员身份登录到 Azure Stack 管理门户

2. 单击“浏览”&gt;“管理资源”&gt;“SQL 宿主服务器”。

    ![](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

    在“SQL 宿主服务器”边栏选项卡上，可将 SQL 服务器资源提供程序连接到充当资源提供程序后端的实际 SQL 服务器实例。

    ![宿主服务器](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. 在表单中填写 SQL Server 实例的连接详细信息。

    ![新宿主服务器](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

      可以选择性地包含实例名称，如果实例未分配给默认端口 1433，则可以提供端口号。

    > [!NOTE]
    > 只要用户和管理员 Azure 资源管理器可以访问 SQL 实例，资源提供程序就能控制此实例。 必须专门将 SQL 实例分配给 RP。

4. 添加服务器时，必须将它们分配给新的或现有的 SKU，以区分服务产品。 例如，可以分配一个 SQL 企业实例来提供：
    - 数据库容量
    - 自动备份
    - 为各个部门保留高性能服务器
    - 等等。

    SKU 名称应反映属性，使用户能够适当地放置其数据库。 SKU 中的所有宿主服务器应有相同的功能。

      示例：

    ![SKU](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

    >[!NOTE]
    > SKU 最长可能需要在一小时后才显示在门户中。 在完全创建 SKU 之前，用户无法创建数据库。

## <a name="provide-capacity-using-sql-always-on-availability-groups"></a>使用 SQL Always On 可用性组提供容量
配置 SQL Always On 实例需要执行附加的步骤，并至少提供三个 VM（或物理机）。

> [!NOTE]
> 对于 Always On，SQL 适配器 RP 仅支持 SQL 2016 SP1 企业版或更高版本的实例，因为它需要新的 SQL 功能，例如自动种子设定。 除了上面所列的一般要求以外，还必须满足以下要求：

- 除了 SQL Always On 计算机以外，还必须提供一个文件服务器。 有一个 [Azure Stack 快速入门模板](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2016-ha)可以创建此环境。 该模板还能引导你构建自己的实例。

- 必须设置 SQL 服务器。 具体而言，必须在每个 SQL Server 实例的每个可用性组上启用[自动种子设定](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group)。

```
ALTER AVAILABILITY GROUP [<availability_group_name>]
    MODIFY REPLICA ON 'InstanceName'
    WITH (SEEDING_MODE = AUTOMATIC)
GO
```

在辅助实例上
```
ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
GO

```



若要添加 SQL Always On 宿主服务器，请遵循以下步骤：

1. 以服务管理员身份登录到 Azure Stack 管理门户

2. 单击“浏览”&gt;“管理资源”&gt;“SQL 宿主服务器”&gt;“+添加”。

    在“SQL 宿主服务器”边栏选项卡上，可将 SQL 服务器资源提供程序连接到充当资源提供程序后端的实际 SQL 服务器实例。


3. 在表单中填写 SQL Server 实例的连接详细信息，并确保使用 Always On 侦听器的 FQDN 或 IPv4 地址（和可选的端口号）。 提供配置的、拥有系统管理员特权的帐户的帐户信息。

4. 选中此框可启用 SQL Always On 可用性组实例的支持。

    ![宿主服务器](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. 将 SQL Always On 实例添加到 SKU。 不能在同一 SKU 中混合使用独立服务器与 Always On 实例。 在添加第一个宿主服务器时需确定其中的一项。 之后尝试混合使用类型会导致出错。


## <a name="making-sql-databases-available-to-users"></a>将 SQL 数据库提供给用户使用

创建计划和产品，使用户能够使用 SQL 数据库。 将 Microsoft.SqlAdapter 服务添加到计划，并添加现有的配额或创建新配额。 如果创建配额，请可以指定允许用户使用的容量。

![创建计划和产品以包含数据库](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)

## <a name="maintenance-of-the-sql-adapter-rp"></a>SQL 适配器 RP 的维护

本文不介绍 SQL 实例的维护，只提供有关密码轮换的信息。 你需要责任修补和备份/恢复与 SQL 适配器配合使用的数据库服务器。

### <a name="patching-and-updating"></a>修补和更新
 不能将 SQL 适配器作为 Azure Stack 的一部分进行维护，因为它是一个加载项组件。 Microsoft 会根据需要为 SQL 适配器提供更新。 SQL 适配器在默认提供程序订阅下面的用户虚拟机上实例化。 因此，需要提供 Windows 修补程序、防病毒签名等信息。使用修补和更新周期提供的 Windows 更新包可将更新应用到 Windows VM。 发布更新的适配器后，会提供一个脚本来应用更新。 此脚本将创建新的 RP VM，并迁移现有的任何状态。

 ### <a name="backuprestoredisaster-recovery"></a>备份/还原/灾难恢复
 不能在 Azure Stack BC-DR 过程中备份 SQL 适配器，因为它是加载项组件。 我们会提供脚本来简化以下操作：
- 备份所需的状态信息（存储在 Azure Stack 存储帐户中）
- 在有必要恢复整个堆栈时还原 RP。
必须先恢复数据库服务器（如果需要），然后再恢复 RP。

### <a name="updating-sql-credentials"></a>更新 SQL 凭据

你需要负责在 SQL 服务器上创建和维护管理员帐户。 RP 需要拥有这些特权的帐户才能代表用户管理数据库 - 它不需要访问这些数据库中的数据。 如果需要更新 SQL 服务器的 sa 密码，可以使用 RP 管理员界面中的更新功能来更改 RP 所用的存储密码。 这些密码存储在 Azure Stack 实例上的 Key Vault 中。

若要修改设置，请单击“浏览”&gt;“管理资源”&gt;“SQL 宿主服务器”&gt;“SQL 登录到”并选择登录名。 必须先在 SQL 实例上（必要时还需要在任何副本上）进行更改。 在“设置”面板中单击“密码”。

![更新管理密码](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)


## <a name="next-steps"></a>后续步骤

[添加数据库](azure-stack-sql-resource-provider-databases.md)

