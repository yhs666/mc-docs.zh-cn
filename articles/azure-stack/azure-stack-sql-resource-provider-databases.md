---
title: 在 Azure Stack 上使用 SQL Adapter RP 提供的数据库 | Microsoft Docs
description: 如何创建和管理使用 SQL 适配器资源提供程序预配的 SQL 数据库
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
origin.date: 10/10/2017
ms.date: 03/04/2018
ms.author: v-junlch
ms.openlocfilehash: 8b9c5d57712b478b35baad07d7aa00664af2c3d2
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="create-sql-databases"></a>创建 SQL 数据库

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

通过用户门户体验提供自助服务数据库。 用户需要包含产品/服务的订阅，该产品/服务应包含数据库服务。

1. 登录到 [Azure Stack](azure-stack-poc.md) 用户门户（服务管理员还可以使用管理门户）。

2. 单击“+ 新建”&gt;“数据 + 存储”&gt;“SQL Server 数据库(预览版)”&gt;“添加”。

3. 在表单中填写数据库详细信息，包括**数据库名称**、**最大大小**，并根据需要更改其他参数。 系统会要求你为数据库选取 SKU。 添加宿主服务器时，将为它们分配 SKU。 将在构成 SKU 的宿主服务器池中创建数据库。

    ![新建数据库](./media/azure-stack-sql-rp-deploy/newsqldb.png)

    >[!NOTE]
    > 数据库大小必须至少为 64 MB。 可以使用设置增加该大小。

4. 填写登录设置：**数据库登录名**和**密码**。 这些设置是仅为访问此数据库创建的 SQL 身份验证凭据。 登录用户名必须全局唯一。 创建新的登录设置，或选择现有登录设置。 可以对使用同一 SKU 的其他数据库重用登录设置。

    ![新建数据库用户名](./media/azure-stack-sql-rp-deploy/create-new-login.png)


5. 提交表单并等待部署完成。

    在显示的边栏选项卡中，请注意“连接字符串”字段。 可以在 Azure Stack 中需要访问 SQL Server 的任何应用程序（例如，Web 应用）中使用该字符串。

    ![检索连接字符串](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="delete-sql-databases"></a>删除 SQL 数据库
在门户中，

>[!NOTE]
>
>从 RP 删除 SQL AlwaysOn 数据库时，会成功从主服务器和 AlwaysOn 可用性组中删除该数据库，但根据设计，SQL AG 会将每个副本中的数据库置于还原中状态，并且不会删除数据库（除非触发删除操作）。 如果未删除数据库，次要副本将转为“未进行同步”状态。 通过 RP 以相同方式重新将新数据库添加到 AG 仍能正常工作。 请参阅[删除辅助数据库](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server)。

## <a name="manage-database-credentials"></a>管理数据库凭据
可以更新数据库凭据（登录设置）。

## <a name="verify-sql-alwayson-databases"></a>验证 SQL AlwaysOn 数据库
AlwaysOn 数据库应显示为已进行同步，并且在所有实例和可用性组中可用。 故障转移后，数据库应该会无缝连接。 可以使用 SQL Server Management Studio 验证数据库是否正在进行同步：

![验证 AlwaysOn](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)

