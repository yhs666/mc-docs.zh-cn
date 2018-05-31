---
title: 在 Azure Stack 中使用 SQL 数据库 | Microsoft Docs
description: 了解如何在 Azure Stack 中部署 SQL 数据库即服务，并通过便捷的步骤部署 SQL Server 资源提供程序适配器。
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
origin.date: 05/01/2018
ms.date: 05/24/2018
ms.author: v-junlch
ms.reviewer: jeffgo
ms.openlocfilehash: 6108e2709ce45100c0080a2e04e29b3ca3adccba
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475109"
---
# <a name="use-sql-databases-on-azure-stack"></a>在 Azure Stack 中使用 SQL 数据库
使用 SQL Server 资源提供程序适配器可以公开 [Azure Stack](azure-stack-poc.md) 的 SQL 数据库即服务。 安装资源提供程序并将它连接到一个或多个 SQL Server 实例之后，你和用户可以创建：
- 适用于云原生应用的数据库。
- 基于 SQL 的网站。
- 基于 SQL 的工作负荷。
无需每次预配托管 SQL Server 的虚拟机 (VM)。

资源提供程序并不支持 [Azure SQL 数据库](https://www.azure.cn/home/features/sql-database/)的所有数据库管理功能。 例如，不会提供弹性数据库池以及自动提升和降低数据库性能的功能。 但是，资源提供程序支持类似的创建、读取、更新和删除 (CRUD) 操作。 API 与 SQL 数据库不兼容。

## <a name="sql-resource-provider-adapter-architecture"></a>SQL 资源提供程序适配器体系结构
资源提供程序由三个组件构成：

- **SQL 资源提供程序适配器 VM**，即运行提供程序服务的 Windows 虚拟机。
- **资源提供程序本身**，处理预配请求并公开数据库资源。
- **托管 SQL 服务器的服务器**，为称作宿主服务器的数据库提供容量。

必须创建一个或多个 SQL Server 实例，并且/或者提供对外部 SQL Server 实例的访问权限。

> [!NOTE]
> 必须通过租户订阅创建安装在 Azure Stack 集成系统上的宿主服务器， 而不能通过默认提供商订阅创建。 必须通过租户门户或者使用相应的登录名通过 PowerShell 会话来创建这些服务器。 所有宿主服务器都是可计费的 VM，并且必须具有相应的许可证。 服务管理员可以是租户订阅的所有者。


## <a name="next-steps"></a>后续步骤

[部署 SQL Server 资源提供程序](azure-stack-sql-resource-provider-deploy.md)

