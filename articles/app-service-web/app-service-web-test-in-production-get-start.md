---
title: "Web 应用的在生产中测试入门"
description: "了解 Azure 应用服务 Web 应用的在生产中测试 (TiP) 功能。"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 4623468d-886e-4203-8012-8f86deb2790b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/13/2016
ms.date: 09/26/2016
ms.author: v-dazen
ms.openlocfilehash: d3b2b0543f0860913e6679c5739587267f98ccf6
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="get-started-with-test-in-production-for-web-apps"></a>Web 应用的在生产中测试入门
在生产中测试（或使用实际客户流量实时测试 Web 应用）是应用开发人员逐渐集成到其[敏捷开发](https://en.wikipedia.org/wiki/Agile_software_development)方法的测试策略。 使用它可以在生产环境中使用实际用户流量测试应用的质量，而不是在测试环境中以综合数据进行测试。 向实际用户开放新的应用，从而了解应用在部署后可能发生的实际问题。 可以根据实际用户流量的容量、速度和变化来确认应用更新的功能、性能和价值，这些都是在测试环境中无法测得的。

## <a name="traffic-routing-in-app-service-web-apps"></a>应用服务 Web 应用中的流量路由
通过 [Azure App Service](/app-service-web/app-service-changes-existing-services) 中的流量路由功能，可以将部分实际用户流量定向到一个或多个[部署槽位](web-sites-staged-publishing.md)，然后使用 [Azure HDInsight](https://www.azure.cn/home/features/hdinsight/) 或 New Relic 等第三方工具来验证更改。 例如，可以使用应用服务实现以下方案：

* 在部署到整个站点之前发现更新中的功能错误或确定性能瓶颈
* 通过在 beta 应用上测量可用性指标，执行更改的“受控试验”
* 逐渐构建出新的更新，并在错误发生时正常恢复为当前版本 
* 在多个部署槽位中运行 [A/B 测试](https://en.wikipedia.org/wiki/A/B_testing)或[多元测试](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing)，优化应用的业务效果

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>在 Web 应用中使用流量路由的要求
* Web 应用必须在标准或高级层运行，因为多个部署槽位有此要求。
* 流量路由需要用户浏览器中的 Cookie 处于启用状态才能正常工作。 客户端会话期间，流量路由使用 Cookie 将客户端浏览器固定到部署槽位。
* 流量路由可通过 Azure PowerShell cmdlet 支持高级 TiP 方案。

## <a name="route-traffic-segment-to-a-deployment-slot"></a>将流量段路由到部署槽位
在每个 TiP 方案的基本级别中，将预定义百分比的实际流量路由到非生产部署槽位。 为此，请执行以下步骤：

> [!NOTE]
> 此处的步骤假设已有[非生产部署槽位](web-sites-staged-publishing.md)，并且部署槽位中[已部署](web-sites-deploy.md)所需的 Web 应用内容。
> 
> 

1. 登录到 [Azure 门户](https://portal.azure.cn/)。
2. 在 Web 应用的边栏选项卡中，单击“设置” > “流量路由”。
   ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. 选择要将流量路由到的槽位以及所需的总流量百分比，然后单击“保存”。

    ![](./media/app-service-web-test-in-production/02-select-slot.png)
4. 转到部署槽位的边栏选项卡。 现在应该可以看到路由到该处的实时流量。

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

配置流量路由后，指定百分比的客户端将随机路由到非生产槽位。 但是请务必注意，客户端自动路由到特定槽位之后，它将在客户端会话期间都“固定”到该槽位。 其实现方法是使用 Cookie 固定用户会话。 如果查看 HTTP 请求，就会发现每个后续请求中都有一个 `TipMix` Cookie。

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>强制将客户端请求发送到特定槽位
除自动流量路由以外，应用服务还可以将请求路由到特定槽位。 如果想要让用户能够选择加入或退出 beta 应用，这就非常有用。 为此，请使用 `x-ms-routing-name` 查询参数。

若要使用 `x-ms-routing-name` 将用户重新路由到特定槽，必须确保该槽已添加到流量路由列表。 由于要显式路由到某个槽位，因此设置的实际路由百分比并不重要。 可根据需要创建“beta 链接”，用户单击该链接即可访问 beta 应用。

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>让用户选择退出 beta 应用
例如，若要让用户选择退出 beta 应用，可以在网页中插入以下链接：

    <a href="<webappname>.chinacloudsites.cn/?x-ms-routing-name=self">Go back to production app</a>

字符串 `x-ms-routing-name=self` 指定生产槽位。 客户端浏览器访问该链接时，不仅会重定向到生产槽，而且每个后续请求都会包含将会话固定到生产槽的 `x-ms-routing-name=self` cookie。

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>让用户选择加入 beta 应用
若要让用户选择加入 beta 应用，请将相同的查询参数设置为非生产槽位的名称，例如：

        <webappname>.chinacloudsites.cn/?x-ms-routing-name=staging

## <a name="more-resources"></a>更多资源
* [为 Azure App Service 中的 Web 应用设置过渡环境](web-sites-staged-publishing.md)
* [通过可预测的方式在 Azure 中部署复杂应用程序](app-service-deploy-complex-application-predictably.md)
* [使用 Azure App Service 进行敏捷软件开发](app-service-agile-software-development.md)
* [对 Web 应用有效使用 DevOps 环境](app-service-web-staged-publishing-realworld-scenarios.md)