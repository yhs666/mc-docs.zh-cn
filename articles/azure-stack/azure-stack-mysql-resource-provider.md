---
title: 将 MySQL 数据库用作 Azure Stack 上的 PaaS | Microsoft Docs
description: 了解如何在 Azure Stack 上部署 MySQL 资源提供程序，并提供 MySQL 数据库即服务。
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
ms.openlocfilehash: 4c50457ed63463cae39d6e8b7f66a198bd86905c
ms.sourcegitcommit: 8a17603589d38b4ae6254bb9fc125d668442ea1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2018
ms.locfileid: "37027290"
---
# <a name="use-mysql-databases-on-azure-stack"></a>在 Azure Stack 上使用 MySQL 数据库 
可以在 Azure Stack 上部署 MySQL 资源提供程序。 在部署资源提供程序之后，可以通过 Azure 资源管理器部署模板创建 MySQL 服务器和数据库。 还可以提供 MySQL 数据库即服务。  

网站上常用的 MySQL 数据库支持许多网站平台。 例如，在部署资源提供程序之后，可以从 Azure Stack 的 Web 应用平台即服务 (PaaS) 加载项创建 WordPress 网站。 
 
若要在无法访问 Internet 的系统上部署 MySQL 提供程序，请将 [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) 文件复制到本地共享。 当系统提示输入共享名时，请提供该共享名。 必须安装 Azure 和 Azure Stack PowerShell 模块。 

## <a name="mysql-server-resource-provider-adapter-architecture"></a>MySQL 服务器资源提供程序适配器体系结构 
资源提供程序由三个组件构成： 
- **MySQL 资源提供程序适配器 VM**，即运行提供程序服务的 Windows 虚拟机。 
- **资源提供程序本身**，处理预配请求并公开数据库资源。 
- **托管 MySQL 服务器的服务器**，为称作宿主服务器的数据库提供容量。 需要自行创建 MySQL 示例，或提供对外部 SQL 实例的访问权限。 请访问 [Azure Stack 快速入门库](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows)，获取可以执行以下操作的示例模板： 
    - 创建 MySQL 服务器。 
    - 从 Azure 市场下载并部署 MySQL 服务器。 


> [!NOTE] 
> 必须通过租户订阅创建安装在 Azure Stack 集成系统上的宿主服务器， 而不能通过默认提供商订阅创建。 必须通过租户门户或者使用相应的登录名通过 PowerShell 会话来创建这些服务器。 所有宿主服务器都是可计费的 VM，并且必须具有相应的许可证。 服务管理员可以是租户订阅的所有者。 

### <a name="required-privileges"></a>所需的特权 
系统帐户必须拥有以下特权： 
- 数据库：创建、删除 
- 登录名：创建、设置、删除、授予、吊销  

## <a name="next-steps"></a>后续步骤
[部署 MySQL 资源提供程序](azure-stack-mysql-resource-provider-deploy.md)

