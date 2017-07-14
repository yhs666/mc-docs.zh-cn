---
title: "安装适用于 SQL 数据仓库的 Visual Studio 和 SSDT | Azure"
description: "安装适用于 Azure SQL 数据仓库的 Visual Studio 和 SQL Server 开发工具 (SSDT)"
services: sql-data-warehouse
documentationcenter: NA
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 0ed9b406-9b42-4fe6-b963-fe0a5b48aac1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
origin.date: 03/30/2017
ms.date: 07/17/2017
ms.author: v-yeche
ms.openlocfilehash: abff4ad391afc215da1853318302d24bdb461563
ms.sourcegitcommit: 3727b139aef04c55efcccfa6a724978491b225a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2017
---
# 安装适用于 SQL 数据仓库的 Visual Studio 和 SSDT
<a id="install-visual-studio-and-ssdt-for-sql-data-warehouse" class="xliff"></a>
若要开发 SQL 数据仓库的应用程序，建议使用最新版本的 Visual Studio，并结合最新版本的 SQL Server Data Tools (SSDT)。  向后兼容也支持装有 SSDT 的 Visual Studio 2013 Update 5。  

使用装有 SSDT 的 Visual Studio 让你可以使用 SQL Server 对象资源管理器以可视化方式浏览 SQL 数据仓库中的表格、视图、存储过程和其他更多对象，并运行查询。

> [!NOTE]
> SQL 数据仓库尚不支持 Visual Studio 数据库项目。  将来的版本会添加此功能。
> 
> 

## 步骤 1：安装 Visual Studio
<a id="step-1-install-visual-studio" class="xliff"></a>
遵循以下链接，下载并安装 Visual Studio。 如果已安装 Visual Studio 2013 或更高版本，请跳到步骤 2 以安装 SSDT。

1. [下载 Visual Studio][]。
2. 按照 MSDN 上的 [安装 Visual Studio][Installing Visual Studio] 指南安装，并选择默认配置。

## 步骤 2：安装 SSDT
<a id="step-2-install-ssdt" class="xliff"></a>
若要安装适用于 Visual Studio 的 SSDT，只需遵循以下步骤从 Visual Studio 检查 SSDT 更新。

1. 在 Visual Studio 中，单击“工具” / “扩展和更新...” / “更新”
2. 选择“**产品更新**”，然后查找“**数据库工具的 Microsoft SQL Server 更新**”

如果找不到更新，则表示已安装最新版本。  若要确认是否已安装 SSDT，请单击“**帮助**” / “**关于 Microsoft Visual Studio**”，然后在列表中查找 SQL Server Data Tools。  SSDT 的最新版本是 14.0.60525.0。  如果 Visual Studio 中的安装选项不可用，你也可以访问 [SSDT 下载][SSDT Download] 页面来手动下载并安装 SSDT。

## 后续步骤
<a id="next-steps" class="xliff"></a>
拥有最新版本的 SSDT 后，便可以[连接][connect]到你的 SQL 数据仓库。

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[下载 Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/zh-cn/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/zh-cn/library/mt204009.aspx