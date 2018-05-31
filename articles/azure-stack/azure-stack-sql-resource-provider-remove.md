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
ms.openlocfilehash: 0da952a4bd3973012555b0cc480d8d4f7a586831
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475110"
---
# <a name="remove-the-sql-resource-provider"></a>删除 SQL 资源提供程序

若要删除 SQL 资源提供程序，必须先删除所有依赖项：

1. 确保已保留针对此 SQL 资源提供程序适配器版本下载的原始部署包。

2. 必须从资源提供程序中删除所有用户数据库。 （删除用户数据库不会删除数据。）此任务应由用户自己执行。

3. 管理员必须从 SQL 资源提供程序适配器中删除宿主服务器。

4. 管理员必须删除引用 SQL 资源提供程序适配器的所有计划。

5. 管理员必须删除与 SQL 资源提供程序适配器关联的所有 SKU 和配额。

6. 使用以下元素重新运行部署脚本：
    - -Uninstall 参数
    - Azure 资源管理器终结点
    - DirectoryTenantID
    - 服务管理员帐户的凭据


