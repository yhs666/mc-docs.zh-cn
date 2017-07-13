---
title: "Azure Web 应用部署常见问题解答 | Azure"
description: "获取 Azure 应用服务 Web 应用功能的部署相关常见问题解答。"
services: app-service\web
documentationcenter: 
author: simonxjx
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
origin.date: 05/16/2017
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: 381d4eb258bd57bf09747f20d9bc7543a460d3f4
ms.sourcegitcommit: b3e981fc35408835936113e2e22a0102a2028ca0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# Azure 中 Web 应用的部署常见问题解答
<a id="deployment-faqs-for-web-apps-in-azure" class="xliff"></a>

本文对 [Azure 应用服务 Web 应用功能](https://www.azure.cn/home/features/app-service/web-apps/)的部署相关常见问题 (FAQ) 进行了解答。

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## 我刚开始使用应用服务 Web 应用。 如何发布代码？
<a id="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code" class="xliff"></a>

可通过以下几种方式发布 Web 应用代码：

*   使用 Visual Studio 进行部署。 如果拥有 Visual Studio 解决方案，请右键单击 Web 应用程序项目，然后选择“发布”。
*   使用 FTP 客户端进行部署。 在 Azure 门户中，下载要向其部署代码的 Web 应用的发布配置文件。 然后，使用相同的发布配置文件 FTP 凭据将文件上传到 \site\wwwroot。

有关详细信息，请参阅[将应用部署到应用服务](web-sites-deploy.md)。

## 尝试从 Visual Studio 进行部署时，收到了一条错误消息。 如何解决此问题？
<a id="i-see-an-error-message-when-i-try-to-deploy-from-visual-studio-how-do-i-resolve-this" class="xliff"></a>

如果看到以下消息，则可能使用的是旧版 SDK：“在资源组 "YourResourceGroup" 中部署资源 "YourResourceName" 时出错: MissingRegistrationForLocation : 未针对位置“中国北部”中的资源类型“组件”注册订阅。 若要访问此位置，请重新注册此提供程序。” 

若要解决此错误，请升级到[最新 SDK](/downloads/)。 如果使用的是最新 SDK 但也收到了此消息，请提交支持请求。

## 如何将 ASP.NET 应用程序从 Visual Studio 部署到应用服务？
<a id="how-do-i-deploy-an-aspnet-application-from-visual-studio-to-app-service" class="xliff"></a>
<a id="deployasp"></a>

[在 Azure 中不到 5 分钟创建你的第一个 ASP.NET Web 应用](/app-service-web/web-sites-dotnet-get-started/)教程介绍如何使用 Visual Studio 2015 将 ASP.NET Web 应用程序部署到应用服务的 Web 应用中。

## 有哪些不同类型的部署凭据？
<a id="what-are-the-different-types-of-deployment-credentials" class="xliff"></a>

应用服务支持两种类型的凭据，分别适用于本地 GIT 部署和 FTP/S 部署。 有关如何配置部署凭据的详细信息，请参阅[为 Azure 应用服务配置部署凭据](app-service-deployment-credentials.md)。

## 应用服务 Web 应用中采用怎样的文件或目录结构？
<a id="what-is-the-file-or-directory-structure-of-my-app-service-web-app" class="xliff"></a>

有应用服务应用的文件结构信息，请参阅 [File structure in Azure](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure)（Azure 中的文件结构）。

## 尝试通过 FTP 传输文件时出现“FTP 错误 550 - 磁盘空间不足”，应如何解决？
<a id="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-the-disk-when-i-try-to-ftp-my-files" class="xliff"></a>

如果看到此消息，磁盘空间可能即将达到 Web 应用服务计划中的磁盘配额。 可能需要根据磁盘空间需求扩展到更高的服务层。 有关定价计划和资源限制的详细信息，请参阅[应用服务定价](https://www.azure.cn/pricing/details/app-service/)。

## 如何为应用服务 Web 应用设置连续部署？
<a id="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app" class="xliff"></a>

可从 GitHub 设置连续部署。 [连续部署到应用服务](app-service-continuous-deployment.md)教程非常有用，它介绍了设置连续部署的方法。

## 如何排查从 GitHub 进行连续部署的问题？
<a id="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github" class="xliff"></a>

有关调查从 GitHub 或 Bitbucket 进行连续部署的问题的帮助，请参阅 [Investigating continuous deployment](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)（调查连续部署）。

## 无法通过 FTP 传输到我的站点并发布代码。 如何解决此问题？
<a id="i-cant-ftp-to-my-site-and-publish-my-code-how-do-i-resolve-this" class="xliff"></a>

若要解决 FTP 问题，请执行以下步骤：

1. 验证是否输入了正确的主机名和凭据。 有关不同类型的凭据及其使用方法的详细信息，请参阅 [Deployment credentials](https://github.com/projectkudu/kudu/wiki/Deployment-credentials)（部署凭据）。
2. 验证防火墙是否阻止了 FTP 端口。 端口应具有以下设置：
    * FTP 控制连接端口：21
    * FTP 数据连接端口：989、10001-10300

## 如何将代码发布到应用服务？
<a id="how-do-i-publish-my-code-to-app-service" class="xliff"></a>

Azure 快速入门旨在帮助用户使用所选的部署堆栈和方法部署应用。 若要使用快速入门，请在 Azure 门户中转到“设置” > “应用部署”。

## 为什么应用有时会在部署到应用服务后重启？
<a id="why-does-my-app-sometimes-restart-after-deployment-to-app-service" class="xliff"></a>

若要了解应用程序部署在哪些情况下可能导致重启，请参阅 [Deployment vs. runtime issues](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts")（部署与运行时问题）。 如本文所述，应用服务将文件部署到 wwwroot 文件夹。 这决不会直接重启应用。

## 如何使用 FTP 或 FTPS 将应用部署到应用服务？
<a id="how-do-i-use-ftp-or-ftps-to-deploy-my-app-to-app-service" class="xliff"></a>

有关如何使用 FTP 或 FTPS 将 Web 应用部署到应用服务的信息，请参阅[使用 FTP/S 将应用部署到应用服务](app-service-deploy-ftp.md)。