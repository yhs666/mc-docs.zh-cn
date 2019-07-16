---
title: 新建 Azure Application Insights 资源 | Azure Docs
description: 为新的实时应用程序手动设置 Application Insights 监视。
services: application-insights
documentationcenter: ''
author: lingliw
manager: digimobile
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 6/4/2019
ms.author: v-lingwu
ms.openlocfilehash: dddf48fbcfcde0a8e16f79d4a8dca3187916ca61
ms.sourcegitcommit: fd927ef42e8e7c5829d7c73dc9864e26f2a11aaa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/04/2019
ms.locfileid: "67562706"
---
# <a name="create-an-application-insights-resource"></a>创建 Application Insights 资源

Azure Application Insights 在 Microsoft Azure *资源*中显示有关应用程序的数据。 因此，创建新资源是[设置 Application Insights 以监视新应用程序][start]中的一个环节。 创建新资源后，可以获取其检测密钥并使用它来配置 Application Insights SDK。 检测密钥会将遥测链接到资源。

## <a name="sign-in-to-microsoft-azure"></a>登录 Microsoft Azure

如果没有 Azure 订阅，请在开始前创建一个[试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。

## <a name="create-an-application-insights-resource"></a>创建 Application Insights 资源

登录 [Azure 门户](https://portal.azure.cn)，并创建 Application Insights 资源：

![单击左上角的“+”号。 选择开发人员工具，然后选择“Application Insights”](./media/create-new-resource/new-app-insights.png)

   | 设置        |  Value           | 说明  |
   | ------------- |:-------------|:-----|
   | **名称**      | 全局唯一值 | 标识所监视的应用的名称。 |
   | **资源组**     | MyResourceGroup      | 用于托管 App Insights 数据的新资源组或现有资源组的名称。 |
   | **Location** | 美国东部 | 选择离你近的位置或离托管应用的位置近的位置。 |

在必填字段中输入适当的值，然后选择“查看 + 创建”  。

![在必填字段中输入值，然后选择“查看 + 创建”。](./media/create-new-resource/review-create.png)

创建应用后，将打开一个新窗格。 可以在此窗格中查看有关受监视应用程序的性能和使用情况数据。 

## <a name="copy-the-instrumentation-key"></a>复制检测密钥

检测密钥标识要将遥测数据与之关联的资源。 需要复制以将检测密钥添加到应用程序的代码中。

![单击并复制检测密钥](./media/create-new-resource/instrumentation-key.png)

## <a name="install-the-sdk-in-your-app"></a>在应用中安装 SDK

在应用中安装 Application Insights SDK。 此步骤在很大程度上依赖于应用程序的类型。

使用检测密钥来配置[在应用程序中安装的 SDK][start]。

SDK 包含无需编写任何其他代码即可发送遥测数据的标准模块。 若要跟踪用户操作或更细致地诊断问题，请[使用 API][api] 发送自己的遥测数据。

## <a name="creating-a-resource-automatically"></a>自动创建资源
可以编写 [PowerShell 脚本](../../azure-monitor/app/powershell.md)来自动创建资源。

## <a name="next-steps"></a>后续步骤
* [诊断搜索](../../azure-monitor/app/diagnostic-search.md)
* [探索指标](../../azure-monitor/app/metrics-explorer.md)
* [编写分析查询](../../azure-monitor/app/analytics.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[start]: ../../azure-monitor/app/app-insights-overview.md





