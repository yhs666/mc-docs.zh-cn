---
title: Azure Stack 上的 MySQL 宿主服务器 | Microsoft Docs
description: 如何添加 MySQL 实例以通过 MySQL 适配器资源提供程序进行预配
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
ms.openlocfilehash: 01e623ff358f8f28af89b82fe6a54f396de75737
ms.sourcegitcommit: 8a17603589d38b4ae6254bb9fc125d668442ea1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2018
ms.locfileid: "37027273"
---
# <a name="add-hosting-servers-for-the-mysql-resource-provider"></a>为 MySQL 资源提供程序添加托管服务器
可以使用 [Azure Stack](azure-stack-poc.md) 内部 VM 上的 MySQL 实例，或者 Azure Stack 环境外部的实例，前提是资源提供程序能够连接到这些实例。 

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>连接到 MySQL 宿主服务器以提供容量 
1. 以服务管理员身份登录到 Azure Stack 门户。 
2. 选择“管理资源” > “MySQL 宿主服务器” > “+添加”。 在“MySQL 宿主服务器”边栏选项卡上，可将 MySQL 服务器资源提供程序连接到充当资源提供程序后端的实际 MySQL 服务器实例。

    ![宿主服务器](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)
  
3. 提供 MySQL 服务器实例的连接详细信息。 请务必提供完全限定域名 (FQDN) 或有效的 IPv4 地址，而不是简短的 VM 名称。 此安装不再提供默认 MySQL 实例。 提供的大小可帮助资源提供程序管理数据库容量。 它应该接近数据库服务器的实际容量。 

    > [!NOTE] 
    > 如果租户和管理 Azure 资源管理器可以访问 MySQL 实例，则资源提供程序可以控制此实例。 必须专门将 SQL 实例分配给资源提供程序。 

4. 添加服务器时，必须将它们分配给新的或现有的 SKU，以区分服务产品。 例如，可以分配一个企业实例来提供： 
    - 数据库容量
    - 自动备份
    - 为各个部门保留高性能服务器 

    > [!IMPORTANT] 
    > 不能在同一 SKU 中混合使用独立服务器与 Always On 实例。 尝试在添加第一个托管服务器后混合类型会导致错误。 

    SKU 名称应反映属性，使租户能够适当地放置其数据库。 SKU 中的所有宿主服务器应有相同的功能。 

    ![创建 MySQL SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png) 


## <a name="add-capacity"></a>添加容量 
在 Azure Stack 门户中添加更多 MySQL 服务器以添加容量。 可将其他服务器添加到新的或现有的 SKU。 确保服务器特征相同。 
 
## <a name="make-mysql-databases-available-to-tenants"></a>将 MySQL 数据库提供给租户使用 
创建计划和套餐，使租户能够使用 MySQL 数据库。 例如，添加 Microsoft.MySqlAdapter 服务、增加配额，等等。 

![创建计划和套餐以包含数据库](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png) 

## <a name="next-steps"></a>后续步骤
[创建 MySQL 数据库](azure-stack-mysql-resource-provider-databases.md)

