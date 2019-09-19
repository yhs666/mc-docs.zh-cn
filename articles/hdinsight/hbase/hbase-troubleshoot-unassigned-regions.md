---
title: Azure HDInsight 中的区域服务器问题
description: Azure HDInsight 中的区域服务器问题
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 08/07/2019
ms.date: 09/23/2019
ms.openlocfilehash: 3f3aa885965c95134c06e6dd2b5da1d402d14941
ms.sourcegitcommit: 43f569aaac795027c2aa583036619ffb8b11b0b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921386"
---
# <a name="issues-with-region-servers-in-azure-hdinsight"></a>Azure HDInsight 中的区域服务器问题

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方法。

## <a name="scenario-unassigned-regions"></a>方案：未分配的区域

### <a name="issue"></a>问题

运行 `hbase hbck` 命令时看到如下所示的错误消息：

```
multiple regions being unassigned or holes in the chain of regions
```

在 Apache HBase Master UI 中，可以看到各区域服务器中的区域计数不平衡。

### <a name="cause"></a>原因

问题可能是区域脱机造成的。

### <a name="resolution"></a>解决方法

修复分配。 执行以下步骤，让未分配区域重新回到正常状态：

1. 使用 SSH 登录到 HDInsight HBase 群集。

1. 运行 `hbase zkcli` 命令连接 Zookeeper shell。

1. 运行 `rmr /hbase/regions-in-transition` 或 `rmr /hbase-unsecure/regions-in-transition` 命令。

1. 使用 `exit` 命令退出 Zookeeper shell。

1. 打开 Ambari UI，从 Ambari 重启活动 HBase Master 服务。

1. 再次运行 `hbase hbck` 命令（不带任何其他选项）。 检查输出并确保正在分配所有区域。

---

## <a name="scenario-dead-region-servers"></a>方案：区域服务器死机

### <a name="issue"></a>问题

区域服务器无法启动。

### <a name="cause"></a>原因

存在多个 WAL 拆分目录。

1. 获取当前 WAL 列表：`hadoop fs -ls -R /hbase/WALs/ > /tmp/wals.out`。

1. 检查 `wals.out` 文件。 如果拆分目录（以 *-splitting 开头）过多，可能会导致区域服务器出现故障。

### <a name="resolution"></a>解决方法

1. 从 Ambari 门户停止 HBase。

1. 执行 `hadoop fs -ls -R /hbase/WALs/ > /tmp/wals.out` 获取 WAL 的最新列表。

1. 将 *-splitting 目录移到临时文件夹 `splitWAL`，并删除 *-splitting 目录。

1. 执行 `hbase zkcli` 命令连接 Zookeeper shell。

1. 执行 `rmr /hbase-unsecure/splitWAL`。

1. 重启 HBase 服务。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
