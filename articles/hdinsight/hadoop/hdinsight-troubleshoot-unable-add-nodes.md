---
title: 无法将节点添加到 Azure HDInsight 群集
description: 无法将节点添加到 Azure HDInsight 群集
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 07/31/2019
ms.date: 09/23/2019
ms.openlocfilehash: ba13c7b473ddc8bdcf5995acf81ed6e271315877
ms.sourcegitcommit: 43f569aaac795027c2aa583036619ffb8b11b0b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921334"
---
# <a name="scenario-unable-to-add-nodes-to-azure-hdinsight-cluster"></a>方案：无法将节点添加到 Azure HDInsight 群集

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

无法将节点添加到 Azure HDInsight 群集。

## <a name="cause"></a>原因

原因可能有所不同。

## <a name="resolution"></a>解决方法

使用[群集大小](../hdinsight-scaling-best-practices.md)功能计算群集所需的附加核心数。 此值基于新辅助角色节点中的核心总数。 然后，请尝试下面的一个或多个步骤：

* 检查以确定群集位置是否有任何核心可用。

* 查看其他位置可用的核心数。 考虑在具有足够可用核心的其他位置重新创建群集。

* 若要提高特定位置的核心配额，请开具一份支持票证，指出想要提高 HDInsight 核心配额。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：


* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
