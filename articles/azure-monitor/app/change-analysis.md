---
title: Azure Monitor 应用程序更改分析 - 使用 Azure Monitor 应用程序更改分析来发现可能影响实时站点问题/中断的更改 | Azure Docs
description: 使用 Azure Monitor 应用程序更改分析排查 Azure 应用服务中的应用程序实时站点问题
services: application-insights
author: lingliw
manager: digimobile
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 6/4/2019
ms.author: v-lingwu
ms.openlocfilehash: b4ebd20c402d43764fd79449ff6aba275960591b
ms.sourcegitcommit: f818003595bd7a6aa66b0d3e1e0e92e79b059868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66732191"
---
# <a name="azure-monitor-application-change-analysis-public-preview"></a>Azure Monitor 应用程序更改分析（公共预览版）

发生实时站点问题/中断时，快速确定根本原因至关重要。 标准的监视解决方案可帮助你快速认识到出现了问题，甚至还会告知哪个组件出现了故障。 但是，它们不一定总能立即解释故障的原因。 你的站点在五分钟前还一切正常，但现在它已中断。 那么，过去 5 分钟发生了什么？ 新功能“Azure Monitor 应用程序更改分析”就是为了解答此问题而开发的。 应用程序更改分析构建在 通过基于的强大功能构建在 [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) 的强大功能基础之上，可让你洞察 Azure 应用服务应用程序的更改，以提高可观察性并减少 MTTR（平均修复时间）。

> [!IMPORTANT]
> Azure Monitor 应用程序更改分析目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。 有关详细信息，请参阅[世纪互联 Azure 预览版补充使用条款](https://www.azure.cn/support/legal/preview-supplemental-terms/)。

## <a name="how-do-i-use-application-change-analysis"></a>如何使用应用程序更改分析？

Azure Monitor 应用程序更改分析目前已内置于自助服务“诊断并解决问题”体验中（可通过 Azure 应用服务应用程序的“概述”部分访问该体验）：  

![Azure 应用服务“概述”页的屏幕截图，红框中显示了“概述”按钮及“诊断并解决问题”按钮](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis"></a>启用更改分析

1. 选择“可用性和性能” 

    ![可用性和性能故障排除选项的屏幕截图](./media/change-analysis/availability-and-performance.png)

2. 单击“应用程序崩溃”磁贴。 

   ![“应用程序崩溃”磁贴的屏幕截图](./media/change-analysis/application-crashes-tile.png)

3. 若要启用“更改分析”，请选择“立即启用”。  

   ![可用性和性能故障排除选项的屏幕截图](./media/change-analysis/application-crashes.png)

4. 若要利用整套更改分析功能，请将“更改分析”、“扫描代码更改”和“始终打开”设置为“打开”，然后选择“保存”。     

    ![Azure 应用服务启用更改分析用户界面的屏幕截图](./media/change-analysis/change-analysis-on.png)

    如果已启用“更改分析”，则可以检测资源级别的更改。  如果已启用“扫描代码更改”，则还可以查看部署文件和站点配置的更改。  启用“始终打开”可优化更改扫描性能，但从计费的角度讲，可能会增大费用。 

5.  启用所有设置后，选择“诊断并解决问题” > “可用性和性能” > “应用程序崩溃”可以访问更改分析体验。    图形将汇总在不同时间发生的更改类型，以及有关这些更改的详细信息：

     ![更改差异视图的屏幕截图](./media/change-analysis/change-view.png)

## <a name="troubleshooting"></a>故障排除

### <a name="unable-to-fetch-change-analysis-information"></a>无法提取更改分析信息。

这是当前预览版载入体验的一个暂时性问题。 解决方法包括在 Web 应用中设置一个隐藏的标记，然后刷新页面：

   ![更改隐藏标记的屏幕截图](./media/change-analysis/hidden-tag.png)

若要使用 PowerShell 设置隐藏的标记：

1. 打开 Azure Cloud Shell：

    ![更改 Azure Cloud Shell 的屏幕截图](./media/change-analysis/cloud-shell.png)

2. 将 shell 类型更改为 PowerShell：

   ![更改 Azure Cloud Shell 的屏幕截图](./media/change-analysis/choose-powershell.png)

3. 运行以下命令：

```powershell
$webapp=Get-AzWebApp -Name <name_of_your_webapp>
$tags = $webapp.Tags
$tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
```

> [!NOTE]
> 添加隐藏的标记后，最初可能仍需要等待最长 4 小时才能查看更改。 这是因为，更改分析服务按 4 小时频率扫描 Web 应用，同时会限制扫描造成的性能影响。

## <a name="next-steps"></a>后续步骤

- 通过[启用 Azure Monitor 的 Application Insights 功能](azure-web-apps.md)来改善 Azure 应用服务的监视。
- 增强对 [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) 的理解，以帮助发挥 Azure Monitor 应用程序更改分析的作用。 




