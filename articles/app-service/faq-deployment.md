---
title: 部署常见问题解答 - Azure 应用服务
description: 获取 Azure 应用服务 Web 应用功能的部署相关常见问题解答。
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
origin.date: 05/11/2018
ms.date: 12/31/2018
ms.author: v-biyu
ms.custom: seodec18
ms.openlocfilehash: 9e057bb0ba477f45a1958517711a2492b76fc624
ms.sourcegitcommit: 80c59ae1174d71509b4aa64a28a98670307a5b38
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735199"
---
# <a name="deployment-faqs-for-web-apps-in-azure"></a>Azure 中 Web 应用的部署常见问题解答

本文对 [Azure 应用服务 Web 应用功能](https://www.azure.cn/home/features/app-service/web-apps/)的部署相关常见问题 (FAQ) 进行了解答。

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-am-just-getting-started-with-app-service-web-apps-how-do-i-publish-my-code"></a>我刚开始使用应用服务 Web 应用。 如何发布代码？

可通过以下几种方式发布 Web 应用代码：

*   使用 Visual Studio 进行部署。 如果具有 Visual Studio 解决方案，则右键单击 Web 应用程序项目，然后选择“发布”。
*   使用 FTP 客户端进行部署。 在 Azure 门户中，下载要向其部署代码的 Web 应用的发布配置文件。 然后，使用相同的发布配置文件 FTP 凭据将文件上传到 \site\wwwroot。

有关详细信息，请参阅[将应用部署到应用服务](deploy-local-git.md)。

## <a name="i-see-an-error-message-when-i-try-to-deploy-from-visual-studio-how-do-i-resolve-this"></a>尝试从 Visual Studio 进行部署时，收到了一条错误消息。 如何解决此问题？

如果看到以下消息，则可能使用的是旧版本的 SDK：“在资源组 'YourResourceGroup' 中部署资源 'YourResourceName' 时出错: MissingRegistrationForLocation: 订阅未在 '中国北部' 位置注册资源类型 'components'。 若要访问此位置，请重新注册此提供程序。” 

若要解决此错误，请升级到[最新 SDK](/downloads/)。 如果使用的是最新 SDK 但也收到了此消息，请提交支持请求。

## <a name="how-do-i-deploy-an-aspnet-application-from-visual-studio-to-app-service"></a>如何将 ASP.NET 应用程序从 Visual Studio 部署到应用服务？
<a id="deployasp"></a>

教程[五分钟内在 Azure 中创建第一个 ASP.NET Web 应用](app-service-web-get-started-dotnet.md)演示如何使用 Visual Studio 2017 将 ASP.NET Web 应用程序部署到应用服务中的 Web 应用。

## <a name="what-are-the-different-types-of-deployment-credentials"></a>有哪些不同类型的部署凭据？

应用服务支持两种类型的凭据，分别适用于本地 GIT 部署和 FTP/S 部署。 有关如何配置部署凭据的详细信息，请参阅[为 Azure 应用服务配置部署凭据](deploy-configure-credentials.md)。

## <a name="what-is-the-file-or-directory-structure-of-my-app-service-web-app"></a>应用服务 Web 应用中采用怎样的文件或目录结构？

有应用服务应用的文件结构信息，请参阅 [File structure in Azure](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure)（Azure 中的文件结构）。

## <a name="how-do-i-resolve-ftp-error-550---there-is-not-enough-space-on-the-disk-when-i-try-to-ftp-my-files"></a>尝试通过 FTP 传输文件时出现“FTP 错误 550 - 磁盘空间不足”，应如何解决？

如果看到此消息，磁盘空间可能即将达到 Web 应用服务计划中的磁盘配额。 可能需要根据磁盘空间需求扩展到更高的服务层。 有关定价计划和资源限制的详细信息，请参阅[应用服务定价](https://www.azure.cn/pricing/details/app-service/)。

## <a name="how-do-i-set-up-continuous-deployment-for-my-app-service-web-app"></a>如何为应用服务 Web 应用设置连续部署？

可以从多个资源（包括 Azure DevOps、OneDrive、GitHub、Bitbucket、Dropbox 和其他 Git 存储库）设置持续部署。 这些选项可在门户中使用。 [持续部署到应用服务](deploy-continuous-deployment.md)是很有帮助的教程，其中说明了如何设置持续部署。

## <a name="how-do-i-troubleshoot-issues-with-continuous-deployment-from-github-and-bitbucket"></a>如何对从 GitHub 和 Bitbucket 进行持续部署的问题进行故障排除？

若要帮助调查从 GitHub 或 Bitbucket 进行持续部署的问题，请参阅[调查持续部署](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)。

## <a name="i-cant-ftp-to-my-site-and-publish-my-code-how-do-i-resolve-this"></a>我无法通过 FTP 传输到我的站点并发布代码。 如何解决此问题？

若要解决 FTP 问题，请执行以下步骤：

1. 验证是否输入了正确的主机名和凭据。 有关不同类型的凭据及其使用方法的详细信息，请参阅 [Deployment credentials](https://github.com/projectkudu/kudu/wiki/Deployment-credentials)（部署凭据）。
2. 验证防火墙是否阻止了 FTP 端口。 端口应具有以下设置：
    * FTP 控制连接端口：21
    * FTP 数据连接端口：989、10001-10300

## <a name="how-do-i-publish-my-code-to-app-service"></a>如何将代码发布到应用服务？

Azure 快速入门旨在帮助用户使用所选的部署堆栈和方法部署应用。 若要使用快速入门，请在 Azure 门户中转到应用服务，在“部署”下，选择“快速入门”。

## <a name="why-does-my-app-sometimes-restart-after-deployment-to-app-service"></a>为什么应用有时会在部署到应用服务后重启？

若要了解应用程序部署在哪些情况下可能导致重启，请参阅 [Deployment vs. runtime issues](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues#deployments-and-web-app-restarts")（部署与运行时问题）。 如本文所述，应用服务将文件部署到 wwwroot 文件夹。 这决不会直接重启应用。

## <a name="how-do-i-integrate-azure-devops-code-with-app-service"></a>如何将 Azure DevOps 代码与应用服务集成？

有两个选项可用于将持续部署与 Azure DevOps 一起使用：

*   使用 Git 项目。 使用该存储库的部署选项，通过应用服务进行连接。
*   使用 Team Foundation 版本控制 (TFVC) 项目。 通过对应用服务使用生成代理来进行部署。

这两个选项的持续代码部署都依赖于现有开发人员工作流和签入过程。 有关详细信息，请参阅以下文章： 

*   [实现应用到 Azure 网站的持续部署](https://www.visualstudio.com/docs/release/examples/azure/azure-web-apps-from-build-and-release-hubs)
*   [设置 Azure DevOps 组织，以便它可以部署到 Web 应用](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)

## <a name="how-do-i-use-ftp-or-ftps-to-deploy-my-app-to-app-service"></a>如何使用 FTP 或 FTPS 将应用部署到应用服务？

有关如何使用 FTP 或 FTPS 将 Web 应用部署到应用服务的信息，请参阅[使用 FTP/S 将应用部署到应用服务](deploy-ftp.md)。

<!--Update_Description: update meta data-->