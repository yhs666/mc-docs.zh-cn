---
title: 使用 FTP/S 将应用部署到 Azure 应用服务 | Azure
description: 了解如何使用 FTP 或 FTPS 将应用部署到 Azure 应用服务。
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/05/2016
ms.date: 10/09/2017
ms.author: v-yiso
ms.openlocfilehash: 5ef5b1271edd910a7c380fcd916ea30230e3e504
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52659944"
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>使用 FTP/S 将应用部署到 Azure 应用服务
本文介绍了如何使用 FTP 或 FTPS 将 Web 应用、移动应用后端或 API 应用部署到 [Azure App Service](/app-service-web/app-service-changes-existing-services)。

应用的 FTP/S 终结点已处于活动状态。 无需配置即可启用 FTP / S 部署。 

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>步骤 1：设置部署凭据

若要访问应用的 FTP 服务器，首先需要部署凭据。 

若要设置或重置部署凭据，请参阅 [Azure App Service 部署凭据](app-service-deployment-credentials.md)。 本教程演示如何使用用户级凭据。

## <a name="step-2-get-ftp-connection-information"></a>步骤 2：获取 FTP 连接信息

1. 在 [Azure 门户](https://portal.azure.cn)中，打开应用的[资源边栏选项卡](../azure-resource-manager/resource-group-portal.md#manage-resources)。
2. 在左侧菜单中选择“概述”，然后记下“FTP/部署用户”、“FTP 主机名”和“FTPS 主机名”的值。 

    ![FTP 连接信息](./media/web-sites-deploy/FTP-Connection-Info.PNG)

    > [!NOTE]
    > Azure 门户中显示了应用名称等“FTP/部署用户”用户值，用于为 FTP 服务器提供适当的上下文。
    > 在左侧菜单中选择“属性”  时，可以找到相同信息。 
    >
    > 此外，永远不会显示部署密码。 如果忘记了部署密码，请返回到 [步骤 1](#step1) ，重置部署密码。
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>步骤 3：将文件部署到 Azure

1. 从 FTP 客户端（[Visual Studio](https://www.visualstudio.com/vs/community/)、[FileZilla](https://filezilla-project.org/download.php?type=client) 等），使用收集到的连接信息连接到应用。
3. 将文件及其各自的目录结构复制到 Azure 中的 [/site/wwwroot 目录](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure)（对于 Web 作业，复制到 /site/wwwroot/App_Data/Jobs/ 目录）。
4. 浏览到应用的 URL，以验证该应用是否正在正常运行。 

> [!NOTE] 
> 与[基于 Git 的部署](app-service-deploy-local-git.md)不同，FTP 部署不支持以下部署自动化： 
>
> - 还原依赖项（如 NuGet、NPM、PIP 和 Composer 自动化）
> - 编译 .NET 二进制文件
> - 生成 web.config（以下提供一个 [Node.js 示例](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps)）
> 
> 必须在本地计算机上手动还原、构建和生成这些必需的文件并将其与应用一起部署。
>
>

## <a name="next-steps"></a>后续步骤

有关更高级的部署方案，请参阅[使用 Git 部署到 Azure](app-service-deploy-local-git.md)。 通过基于 Git 的 Azure 部署可实现版本控制、包还原、MSBuild 等功能。

## <a name="more-resources"></a>更多资源

* [ 部署凭据](app-service-deploy-ftp.md)