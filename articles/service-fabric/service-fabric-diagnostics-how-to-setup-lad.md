---
title: "使用 Linux Azure 诊断收集日志 | Azure"
description: "本文介绍如何将 Azure 诊断设置为从 Azure 中运行的 Service Fabric Linux 群集收集日志。"
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: a160d469-8b7d-4560-82dd-8500db34a44a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 6/28/2017
ms.date: 08/14/2017
ms.author: v-yeche
ms.openlocfilehash: db916254690fdc806f0b9ff089b215cfdfceccc2
ms.sourcegitcommit: c36484a7fdbe4b85b58179d20d863ab16203b6db
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a>使用 Azure 诊断收集日志
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
> * [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

运行 Azure Service Fabric 群集时，最好是从一个中心位置的所有节点中收集日志。 将日志放在中心位置，无论问题发生在服务中、应用程序中还是群集本身，都能轻松分析和排查问题。 上传和收集日志的方式之一是使用可将日志上传到 Azure 存储或 Azure 事件中心的 Azure 诊断扩展。 也可从存储或事件中心读取事件。
<!-- Not Available [Azure Application Insights], [Log Analytics](../log-analytics/log-analytics-service-fabric.md)  -->

## <a name="log-sources-that-you-might-want-to-collect"></a>想要收集的日志源
* 
            **Service Fabric 日志**：由平台通过 [LTTng](http://lttng.org) 发出，上传到存储帐户。 日志可能是平台发出的操作事件或运行时事件。 这些日志存储在群集清单指定的位置。 （如果要获取存储帐户的详细信息，请搜索 **AzureTableWinFabETWQueryable** 标记，并查找 **StoreConnectionString**。）
* **应用程序事件**：从服务代码发出。 可以使用任何能够写入基于文本的日志的日志记录解决方案，例如 LTTng。 有关详细信息，请参阅有关跟踪应用程序的 LTTng 文档。  

## <a name="deploy-the-diagnostics-extension"></a>部署诊断扩展
收集日志的第一个步骤是将诊断扩展部署在 Service Fabric 群集中的每个 VM 上。 诊断扩展会收集每个 VM 上的日志，并将它们上传到指定的存储帐户。 根据使用的是 Azure 门户还是 Azure Resource Manager，步骤有所不同。

要在创建群集期间将诊断扩展部署到群集中的 VM，请将“**诊断**”设置为“**打开**”。 创建群集后，无法使用门户更改此设置。

然后，配置 Linux Azure 诊断 (LAD) 来收集文件并将其放入用户的存储帐户。 [使用 LAD 监视和诊断 Linux VM](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) 一文中的方案 3（“上传自己的日志文件”）说明了此过程。 遵循此过程即可访问跟踪。 可以将跟踪上传到所选的可视化工具。

也可以使用 Azure Resource Manager 部署诊断扩展。 Windows 和 Linux 上的部署过程类似，[如何使用 Azure 诊断收集日志](service-fabric-diagnostics-how-to-setup-wad.md)中介绍了适用于 Windows 群集的过程。

也可以按 [Operations Management Suite Log Analytics with Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/)（在 Linux 中使用 Operations Management Suite Log Analytics）中所述使用 Operations Management Suite。

完成此配置后，LAD 代理将监视指定的日志文件。 每当在文件中追加新行时，该代理都会创建一个 syslog 条目并将其发送到指定的存储。

## <a name="next-steps"></a>后续步骤
若要更详细了解在排查问题时应检查哪些事件，请参阅 [LTTng 文档](http://lttng.org/docs)和[使用 LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)。

<!--Update_Description: update meta properties, update reference link -->