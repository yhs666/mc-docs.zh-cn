---
title: 如何删除 HDInsight 群集 - Azure | Azure
description: 删除 HDInsight 群集的各种方式的相关信息。
services: hdinsight
documentationcenter: ''
author: jasonwhowell
manager: cgronlun
editor: cgronlun
ms.assetid: 55f7838b-9786-47ff-96db-1b64437bd0bb
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 03/22/2018
ms.date: 11/19/2018
ms.author: v-yiso
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 7367fc789a42b2b66d2e4c549ca581d7c024b1d8
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52659287"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-classic-cli"></a>使用浏览器、PowerShell 或 Azure 经典 CLI 删除 HDInsight 群集

HDInsight 群集计费在创建群集之后便会开始，删除群集后才会停止。 HDInsight 群集按分钟收费，因此不再需要使用群集时，应将其删除。 本文档介绍如何使用 Azure 门户、Azure PowerShell 和 Azure 经典 CLI 删除群集。

> [!IMPORTANT]
> 删除 HDInsight 群集不会删除与该群集关联的 Azure 存储帐户。 可重新使用以后存储在这些服务中的数据。

## <a name="azure-portal"></a>Azure 门户

1. 登录 [Azure 门户](https://portal.azure.cn)，并选择 HDInsight 群集。 如果 HDInsight 群集未固定到仪表板，可使用搜索字段按名称进行搜索。

    ![门户搜索](./media/hdinsight-delete-cluster/navbar.png)

2. 在群集设置中，选择“删除”图标。 出现提示时，选择“是”  即可删除该群集。
   
    ![删除图标](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

在 PowerShell 提示符处，使用以下命令删除群集：

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

将 **CLUSTERNAME** 替换为 HDInsight 群集的名称。

## <a name="azure-classic-cli"></a>Azure 经典 CLI

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

在提示符处，使用以下命令删除群集：

    azure hdinsight cluster delete CLUSTERNAME

将 **CLUSTERNAME** 替换为 HDInsight 群集的名称。

> [!NOTE]
> Azure CLI 2.0 暂不支持删除 HDInsight 群集（2017 年 10 月 23 日）。
<!--Update_Description: update words: add a note-->