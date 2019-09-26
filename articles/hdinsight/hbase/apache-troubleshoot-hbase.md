---
title: 使用 Azure HDInsight 对 HBase 进行故障排除 | Microsoft Docs
description: 获取有关使用 HBase 和 Azure HDInsight 的常见问题的解答。
services: hdinsight
documentationcenter: ''
author: nitinver
manager: ashitg
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 08/14/2019
ms.date: 09/23/2019
ms.author: v-yiso
ms.openlocfilehash: 97be57fd75a7211cdcfb932373119a568914fd27
ms.sourcegitcommit: 43f569aaac795027c2aa583036619ffb8b11b0b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921214"
---
# <a name="troubleshoot-apache-hbase-by-using-azure-hdinsight"></a>使用 Azure HDInsight 对 Apache HBase 进行故障排除

了解处理 Apache Ambari 中的 Apache HBase 有效负载时的最常见问题及其解决方法。

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>如何对多个未分配区域运行 hbck 命令报告？

运行 `hbase hbck` 命令时，可能会出现的一条常见错误消息是“多个区域未分配，或者区域链中出现漏洞”。

在 HBase Master UI 中，可以看到所有区域服务器中非均衡区域的数目。 然后，可以运行 `hbase hbck` 命令查看区域链中的漏洞。

漏洞可能是脱机区域造成的，因此请先修复分配问题。 

若要使未分配的区域恢复正常状态，请完成以下步骤：

1. 使用 SSH 登录到 HDInsight HBase 群集。
2. 若要与 Apache ZooKeeper shell 进行连接，请运行 `hbase zkcli` 命令。
3. 运行 `rmr /hbase/regions-in-transition` 命令或 `rmr /hbase-unsecure/regions-in-transition` 命令。
4. 若要从 `hbase zkcli` shell 退出，请使用 `exit` 命令。
5. 打开 Apache Ambari UI，并重启 Active HBase Master 服务。
6. 再次运行 `hbase hbck` 命令（不带任何选项）。 检查此命令的输出以确保分配了所有区域。


## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>使用 hbck 命令进行区域分配时，如何解决超时问题？

### <a name="issue"></a>问题

使用 `hbck` 命令时出现超时问题的可能原因是多个区域长时间处于“正在转换”状态。 在 HBase Master UI 中可以看到这些区域显示为脱机。 由于有大量区域正在尝试进行转换，因此，HBase Master 可能会超时并且无法使那些区域恢复联机。

### <a name="resolution-steps"></a>解决步骤

1. 使用 SSH 登录到 HDInsight HBase 群集。
2. 若要与 Apache ZooKeeper shell 进行连接，请运行 `hbase zkcli` 命令。
3. 运行 `rmr /hbase/regions-in-transition` 或 `rmr /hbase-unsecure/regions-in-transition` 命令。
4. 若要退出 `hbase zkcli` shell，请使用 `exit` 命令。
5. 在 Ambari UI 中，重启 Active HBase Master 服务。
6. 再次运行命令 `hbase hbck -fixAssignments`。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
