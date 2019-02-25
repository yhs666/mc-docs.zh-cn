---
title: 为基于 Linux 的 HDInsight 群集配置 OS 修补计划 - Azure | Azure
description: 了解如何为基于 Linux 的 HDInsight 群集配置 OS 修补计划。
services: hdinsight
documentationcenter: ''
author: bprakash
manager: asadk
editor: bprakash
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 01/24/2019
ms.date: 03/04/2019
ms.author: v-yiso
ms.openlocfilehash: 686d5a30762957ce5267168868e9b75e998c13c7
ms.sourcegitcommit: 0fd74557936098811166d0e9148e66b350e5b5fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56665637"
---
# <a name="os-patching-for-hdinsight"></a>针对 HDInsight 的 OS 修补 

> [!IMPORTANT]
> Ubuntu 映像可在发布后的 3 个月内用于创建新的 HDInsight 群集。 自 2019 年 1 月起，系统**不**会自动修补正在运行的群集。 客户必须使用脚本操作或其他机制来修补正在运行的群集。 新创建的群集将始终包含最新的可用更新，其中包括最新的安全修补程序。

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>如何为基于 Linux 的 HDInsight 群集配置 OS 修补计划
需不定期重启 HDInsight 群集中的虚拟机，以便安装重要的安全修补程序。 

可以使用本文中介绍的脚本操作将 OS 修补计划修改如下：
1. 启用或禁用自动重启
2. 设置重新启动的频率（两次重新启动相隔的天数）
3. 设置在星期几重新启动

> [!NOTE]
> 此脚本操作仅适用于 2016 年 8 月 1 日后创建的基于 Linux 的 HDInsight 群集。 修补程序在 VM 重新启动后才生效。 
>

## <a name="how-to-use-the-script"></a>如何使用脚本 

使用此脚本需要以下信息：
1. 脚本位置： https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh。HDInsight 使用此 URI 在群集中的所有虚拟机上查找并运行脚本。

2. 脚本所适用的群集节点类型：头节点、辅助角色节点、zookeeper。 此脚本必须适用于群集中的所有节点类型。 如果此脚本不适用于某个节点类型，则该节点类型的虚拟机会继续使用以前的修补计划。

3.  参数：此脚本接受三个数字参数：

    | 参数 | 定义 |
    | --- | --- |
    | 启用/禁用自动重新启动 |0 或 1。 值为 0 表示禁用自动重启，而值为 1 则表示启用自动重启。 |
    | 频率 |7 到 90（含）。 在重新启动虚拟机之前需等待的天数，适用于需要重新启动的修补程序。 |
    | 星期几 |1 到 7（含）。 值为 1 表示应在星期一重新启动，值为 7 表示应在星期日重新启动。例如，使用参数 1 60 2 时，至多每隔 60 天就会在星期二自动重新启动。 |
    | 持久性 |向现有群集应用脚本操作时，可将脚本标记为持久性脚本。 通过缩放操作将新的辅助角色节点添加到群集时，会应用持久性脚本。 |

> [!NOTE]
> 在应用到现有群集时，必须将该脚本标记为持久性脚本。 否则，任何通过缩放操作创建的新节点都会使用默认的修补计划。  如果在群集创建过程中应用该脚本，则其会自动持久化。
>

## <a name="next-steps"></a>后续步骤

若要了解使用脚本操作的具体步骤，请参阅[使用脚本操作自定义基于 Linux 的 HDInsight 群集](hdinsight-hadoop-customize-cluster-linux.md)中的以下部分：

* [在创建群集期间使用脚本操作](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [将脚本操作应用到正在运行的群集](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
