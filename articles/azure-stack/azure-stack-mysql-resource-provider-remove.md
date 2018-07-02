---
title: 在 Azure Stack 上删除 MySQL 资源提供程序 | Microsoft Docs
description: 了解如何从 Azure Stack 部署中删除 MySQL 资源提供程序。
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
ms.openlocfilehash: 0a2aa6bd5d7ad1e4baf0f8decf99619292c295ac
ms.sourcegitcommit: 8a17603589d38b4ae6254bb9fc125d668442ea1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2018
ms.locfileid: "37027304"
---
# <a name="removing-the-mysql-resource-provider"></a>删除 MySQL 资源提供程序  
在删除 MySQL 资源提供程序之前，必须先删除所有依赖项。

## <a name="remove-the-mysql-resource-provider"></a>删除 MySQL 资源提供程序 

1. 确认已删除所有现有的 MySQL 资源提供程序依赖项。

    > [!NOTE]
    > 即使依赖资源当前正在使用 MySQL 资源提供程序，也将继续卸载该资源提供程序。 
  
2. 确保已保留针对此 SQL 资源提供程序适配器版本下载的原始部署包。

3. 必须从资源提供程序中删除所有租户数据库。 （删除租户数据库不会删除数据。）此任务应由租户自己执行。 

4. 必须从命名空间中取消注册租户。 

5. 管理员必须从 MySQL 适配器中删除宿主服务器。 

6. 管理员必须删除引用 MySQL 适配器的所有计划。

7. 管理员必须删除与 MySQL 适配器关联的所有配额。 

8. 使用以下参数重新运行部署脚本： 
    - -Uninstall 参数 
    - Azure 资源管理器终结点 
    - DirectoryTenantID 
    - 服务管理员帐户的凭据 

## <a name="next-steps"></a>后续步骤
[提供应用服务作为 PaaS](azure-stack-app-service-overview.md)

