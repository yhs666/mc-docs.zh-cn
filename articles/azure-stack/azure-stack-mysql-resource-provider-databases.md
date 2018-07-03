---
title: 在 Azure Stack 上使用 MySQL Adapter RP 提供的数据库 | Microsoft Docs
description: 如何创建和管理使用 MySQL 适配器资源提供程序预配的 MySQL 数据库
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/14/2018
ms.date: 06/26/2018
ms.author: v-junlch
ms.reviewer: jeffgo
ms.openlocfilehash: bace4876b0169718f47bb42ad2ac50b94f45b7e0
ms.sourcegitcommit: 8a17603589d38b4ae6254bb9fc125d668442ea1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2018
ms.locfileid: "37027271"
---
# <a name="create-mysql-databases"></a>创建 MySQL 数据库
通过用户门户提供自助服务数据库。 Azure Stack 用户需要一个包含套餐的订阅，该套餐应包含 MySQL 数据库服务。
 
## <a name="test-your-deployment-by-creating-your-first-mysql-database"></a>创建第一个 MySQL 数据库以测试部署 
1. 以服务管理员身份登录到 Azure Stack 门户。 
2. 选择“+ 新建” > “数据 + 存储” > “MySQL 数据库”。 
3. 提供数据库详细信息。 

    ![创建 MySQL 测试数据库](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)
 
4. 选择 SKU。 

    ![选择 SKU](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png) 

5. 创建登录设置。 可以重复使用现有的登录设置，或创建新的登录设置。 此设置包含数据库的用户名和密码。 

    ![新建数据库用户名](./media/azure-stack-mysql-rp-deploy/create-new-login.png) 

    连接字符串包含实际的数据库服务器名称。 请从门户复制连接字符串。 

    ![获取 MySQL 数据库的连接字符串](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png) 

    > [!NOTE] 
    > MySQL 5.7 中的用户名长度不可超过 32 个字符。 在早期版本中不可超过 16 个字符。 

## <a name="update-the-administrative-password"></a>更新管理密码 
若要修改密码，可以先在 MySQL 服务器实例上更改密码。 选择“管理资源” > “MySQL 宿主服务器”。 然后选择宿主服务器。 在“设置”面板中选择“密码”。 

![更新管理密码](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png) 

## <a name="next-steps"></a>后续步骤
[更新 MySQL 资源提供程序](azure-stack-mysql-resource-provider-update.md)

