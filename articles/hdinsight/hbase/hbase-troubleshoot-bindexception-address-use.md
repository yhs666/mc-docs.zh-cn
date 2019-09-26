---
title: BindException - 地址已在 Azure HDInsight 中使用
description: BindException - 地址已在 Azure HDInsight 中使用
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 08/08/2019
ms.date: 09/23/2019
ms.openlocfilehash: dfc6b96d0136f6fa3ec9f43f1bc6cd76da46041e
ms.sourcegitcommit: 43f569aaac795027c2aa583036619ffb8b11b0b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921374"
---
# <a name="scenario-bindexception---address-already-in-use-in-azure-hdinsight"></a>方案：BindException - 地址已在 Azure HDInsight 中使用

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

针对 Apache HBase 区域服务器执行的重启操作无法完成。 从区域服务器启动失败的工作器节点上 `/var/log/hbase` 目录中的 `region-server.log`，可能会看到如下错误消息：

```
Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
...

Caused by: java.net.BindException: Address already in use
...
```

## <a name="cause"></a>原因

在高工作负荷活动期间重启 HBase 区域服务器。 以下是当用户通过 Ambari UI 在 HBase 区域服务器上发起重启操作时，后台发生的情况：

1. Ambari 代理将向区域服务器发送停止请求。

1. 然后，Ambari 代理等待 30 秒，让区域服务器正常关闭。

1. 如果应用程序继续与区域服务器进行连接，则区域服务器不会立即关闭，因此，30 秒的超时将很快到期。

1. 在 30 秒到期后，Ambari 代理将向区域服务器发送强制终止命令 (kill -9)。

1. 由于此关闭很突然，尽管区域服务器进程已被终止，但与该进程关联的端口可能还没有释放，这最终会导致 `AddressBindException`。

## <a name="resolution"></a>解决方法

在发起重启之前，减少 HBase 区域服务器上的负载。

或者，尝试使用以下命令，手动重启工作器节点上的区域服务器：

```bash
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"
```

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道之一获取更多支持：


* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 Microsoft Azure 订阅包含对订阅管理和计费支持的访问权限，并且通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
