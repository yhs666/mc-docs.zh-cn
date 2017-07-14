---
title: "Microsoft Azure 中的监视 | Azure"
description: "用于监视 Azure 中任何组件的选项。 Azure Monitor, Application Insights Log Analytics"
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: v-yiso
ms.openlocfilehash: 60728d2fe921647bba62eff9a3a8925770230157
ms.sourcegitcommit: 6728c686935e3cdfaa93a7a364b959ab2ebad361
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# Microsoft Azure 中的监视概述
<a id="overview-of-monitoring-in-microsoft-azure" class="xliff"></a>
本文概述可用于监视 Microsoft Azure 的工具。 本文的内容适用于 
- Microsoft Azure 中运行的监视应用程序 
- 在 Azure 中外部运行的，可监视 Azure 中的对象的工具/服务。 

本文讨论了各种可用的服务和产品以及它们如何配合工作。 本文可帮助你确定在哪些情况情况最适合使用哪些工具。  

## 为何要使用监视和诊断？
<a id="why-use-monitoring-and-diagnostics" class="xliff"></a>

云应用中的性能问题可能会影响业务。 使用多个互连的组件和频繁发布版本时，性能随时可能会下降。 开发一款应用后，你的用户通常会发现其中的问题，而你在测试时却找不到这样的问题。 你应该立即知道这些问题，并使用工具来诊断和解决问题。 Microsoft Azure 提供一系列的工具用于识别这些问题。

## 如何监视 Azure 云应用？
<a id="how-do-i-monitor-my-azure-cloud-apps" class="xliff"></a>

可以使用一系列工具来监视 Azure 应用程序和服务。 这些工具的某些功能是相互重叠的。 之所以存在这种重叠，一部分原因是历史遗留问题，还有一部分原因是应用程序的开发与操作之间界限不明。 

下面是主要工具：

-   **Azure Monitor** 是用于监视 Azure 中运行的服务的基本工具。 它可以提供有关服务吞吐量和周边环境的基础结构级数据。 如果你在 Azure 中管理所有应用，并想要确定是否需要扩展或缩减资源，则 Azure Monitor 可以提供初始信息。

-   **Application Insights** 可用于开发，并且可充当生产监视解决方案。 它的工作方式是在应用中安装一个包，提供更内在的动态视图。 其数据包括依赖项的响应时间、异常跟踪、调试快照和执行配置文件。 它提供强大的智能工具用于分析所有这些遥测数据，既能帮助你调试应用，也能帮助你了解用户将它用于哪种目的。 你可以判断响应时间出现尖峰的原因是应用中出现某种问题，还是存在某种外部资源调配问题。 如果你使用 Visual Studio，而应用出现错误，你可以直接找到有问题的代码行，然后修复错误。  

-   **Log Analytics** 面向需要优化性能，并针对生产环境中运行的应用程序规划维护的用户。 此服务驻留在 Azure 中。 它可以从许多源收集和聚合数据，不过会出现 10 到 15 分钟的延迟。 它为 Azure、本地基础结构和第三方基于云的基础结构（例如 Amazon Web Services）提供整体 IT 管理解决方案。 它提供更丰富的工具来跨更多的源分析数据，允许针对所有日志运行复杂的查询，并可以根据指定的条件主动发出警报。  甚至可以将自定义数据收集到它的中心存储库，以便查询和可视化这些数据。 

-   **System Center Operations Manager (SCOM)** 用于管理和监视大型云安装。 作为本地 Windows Sever 和基于云的 Hyper-V 计算机的管理工具，你可能已经对它有所了解，但它还能集成和管理 Azure 应用。 除此之外，它还能在现有的实时应用中安装 Application Insights。  如果某个应用出现故障，它会即时发出通知。 请注意，Log Analytics 不能取代 SCOM， 但两者可以完美配合工作。  


## 在 Azure 门户中访问监视功能
<a id="accessing-monitoring-in-the-azure-portal" class="xliff"></a>
所有 Azure 监视服务目前在单个 UI 窗格中提供。 

还可以访问特定资源的监视功能，只需突出显示这些资源，然后向下钻取到其监视选项即可。 

## 有关何时使用哪种工具的示例
<a id="examples-of-when-to-use-which-tool" class="xliff"></a> 

以下部分介绍了一些基本方案，以及应该将哪些工具结合使用。 

### 方案 1 – 修复开发中的 Azure 应用程序中的错误
<a id="scenario-1--fix-errors-in-an-azure-application-under-development" class="xliff"></a>   

**最好是结合使用 Application Insights、Azure Monitor 和 Visual Studio**

Azure 现在可以在云中提供 Visual Studio 调试器的完整功能。 将 Azure Monitor 配置为向 Application Insights 发送遥测数据 让 Visual Studio 在应用程序中包含 Application Insights SDK。 进入 Application Insights 后，可以使用应用程序映射来直观了解正在运行的应用程序的哪些部件是否正常。 对于不正常的部件，将会提供错误和异常用于调查。 可以使用 Application Insights 中的各种分析功能进行更深入的分析。 如果不确定错误的原因所在，可以使用 Visual Studio 调试器来跟踪代码，进一步查明问题。 


### 方案 2 – 调试 Azure.NET Web 应用程序，查明只会出现在生产环境中的错误
<a id="scenario-2--debug-an-azure-net-web-application-for-errors-that-only-show-in-production" class="xliff"></a> 

> [!NOTE]
> 这些功能目前以预览版提供。 

**最好是使用 Application Insights，在可能的情况下，还可以使用 Visual Studio 来获得完整的调试体验。**

使用 Application Insights 快照调试器调试应用。 如果生产组件出现某个特定的错误阈值，系统将在称为“快照”的时间窗口内自动捕获遥测数据。 捕获的数据量对于生产云而言是安全的，因为这种数量非常小，还不足以影响性能，但却远远超过了跟踪所需的数量。  系统可以捕获多个快照。 可以在 Azure 门户中查看时间点，或使用 Visual Studio 获得完整体验。 使用 Visual Studio 时，开发人员可以从头到尾浏览快照，就像实时调试一样。 本地变量、参数、内存和帧都可使用。 必须通过 RBAC 角色向开发人员授予对这些生产数据的访问权限。  


### 方案 3 – 调试使用容器或微服务的 Azure 应用程序
<a id="scenario-3--debug-an-azure-application-that-uses-containers-or-microservices" class="xliff"></a> 

**与方案 1 相同。结合使用 Application Insights、Azure Monitor 和 Visual Studio。**Application Insights 还支持从容器中运行的进程以及从微服务（Kubernetes、Docker、Azure Service Fabric）收集遥测数据。 有关详细信息，[请观看这段有关调试容器和微服务的视频](https://go.microsoft.com/fwlink/?linkid=848184)。 





## 后续步骤
<a id="next-steps" class="xliff"></a>
详细了解以下内容

* [Ignite 2016 的 Azure Monitor 视频](https://myignite.microsoft.com/videos/4977)
* [Azure 诊断](../azure-diagnostics.md)：如果要尝试诊断云服务、虚拟机、虚拟机规模集或 Service Fabric 应用程序中的问题。
