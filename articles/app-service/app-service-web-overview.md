---
title: Web 应用概述 | Azure
description: 了解 Azure 应用服务如何帮助用户开发和托管 Web 应用程序
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
origin.date: 01/04/2017
ms.date: 10/08/2018
ms.author: v-yiso
ms.custom: mvc
ms.openlocfilehash: bc85653bda546d0c5d7194686da8b8b86b027292
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52660557"
---
# <a name="web-apps-overview"></a>Web 应用概述

*Azure 应用服务 Web 应用*（简称 Web 应用）是用于托管 Web 应用程序、REST API 和移动后端的服务。 可以使用 .NET、NET Core、Java、Ruby、Node.js、PHP 或 Python 等偏好的语言进行开发。 

Web 应用不仅可将 Microsoft Azure 的强大功能（例如安全性、负载均衡、自动缩放和自动管理）添加到应用程序， 还可以利用其 DevOps 功能，例如从 Azure DevOps、GitHub、Docker 中心和其他源持续部署，以及包管理、过渡环境、自定义域和 SSL 证书。 

使用应用服务时，需要支付 Azure 计算资源的使用费。 使用的计算资源量由运行 Web 应用的应用服务计划确定。 有关详细信息，请参阅 [Azure Web 应用中的应用服务计划](azure-web-sites-web-hosting-plans-in-depth-overview.md)。


## <a name="why-use-web-apps"></a>为何使用 Web 应用？
下面是应用服务 Web 应用的一些重要功能：

* **多个语言和框架** - Web 应用针对 ASP.NET、ASP.NET Core、Java、Ruby、Node.js、PHP 或 Python 提供一流支持。 我们还能以后台服务的形式运行 [PowerShell 和其他脚本或可执行文件](web-sites-create-web-jobs.md)。
* **DevOps 优化** - 使用 Azure DevOps、GitHub 设置[持续集成和部署](app-service-continuous-deployment.md)。 通过 [测试和过渡环境](web-sites-staged-publishing.md)提升更新。 在 Web 应用中使用 [Azure PowerShell](/powershell/azureps-cmdlets-docs) 或[跨平台命令行接口 (CLI)](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-lastest) 管理应用。
* **具有高可用性的全局缩放** - 以手动或自动方式进行[增大](web-sites-scale.md)或[扩大](../monitoring-and-diagnostics/insights-how-to-scale.md)。 在 Azure.cn 的全国数据中心基础结构中的任意位置托管应用，并且应用服务 [SLA](https://www.azure.cn/support/sla/app-service/) 承诺高可用性。
* **安全性和合规性** - 应用服务符合 [ISO、SOC 和 PCI](https://www.microsoft.com/en-us/trustcenter)的要求。 使用 [Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md) 和 [Microsoft](app-service-mobile-how-to-configure-microsoft-authentication.md)) 对用户进行身份验证。 创建 [IP 地址限制](app-service-ip-restrictions.md)。
* **Visual Studio 集成** — Visual Studio 中的专用工具可简化创建、部署和调试工作。
* **API 和移动功能** - Web 应用针对 RESTful API 方案提供统包式 CORS 支持，通过启用身份验证、脱机数据同步、推送通知等功能简化移动应用方案。
* **无服务器代码** - 按需运行代码片段或脚本，无需显式预配或管理基础结构，并且只需为代码实际使用的计算时间付费（请参阅 [Azure Functions](/azure-functions/)）。

除了应用服务中的 Web 应用，Azure 还提供可用来托管网站和 Web 应用程序的其他服务。 大多数情况下，Web 应用是最佳选择。  对于微服务体系结构，请考虑使用 [Service Fabric](/service-fabric)。 如果需要更好地控制运行代码的 VM，请考虑使用 [Azure 虚拟机](/virtual-machines/)。 有关如何在这些 Azure 服务之间做出选择的详细信息，请参阅 [Azure 应用服务、虚拟机、Service Fabric 和云服务的比较](choose-web-site-cloud-service-vm.md)。

## <a name="next-steps"></a>后续步骤

创建第一个 Web 应用。

> [!div class="nextstepaction"]
> [ASP.NET Core](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet-framework.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)

> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)

> [!div class="nextstepaction"]
> [Python](app-service-web-get-started-python.md)

> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)

