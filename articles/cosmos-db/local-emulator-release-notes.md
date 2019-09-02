---
title: Azure Cosmos 模拟器下载和发行说明
description: 阅读 Azure Cosmos 模拟器发行说明并下载。
ms.service: cosmos-db
ms.topic: tutorial
author: rockboyfor
ms.author: v-yeche
origin.date: 06/20/2019
ms.date: 07/29/2019
ms.openlocfilehash: 2c2fa2acd1907ace308a0bd1a344891c2380daee
ms.sourcegitcommit: 5a4a826eea3914911fd93592e0f835efc9173133
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2019
ms.locfileid: "68672311"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>使用 Azure Cosmos 模拟器进行本地开发和测试

本文显示了 Azure Cosmos 模拟器发行说明，其中包含每个发行版中所做的功能更新列表。 它还列出了要下载和使用的模拟器的最新版本。

## <a name="download"></a>下载

| | |
|---------|---------|
|**MSI 下载**|[下载中心](https://aka.ms/cosmosdb-emulator)|
|**入门**|[使用 Azure Cosmos 模拟器在本地开发](local-emulator.md)|

## <a name="release-notes"></a>发行说明

### <a name="243"></a>2.4.3

- 默认情况下已禁止启动 MongoDB 服务。 默认情况下仅启用 SQL 终结点。 用户必须使用模拟器的“/EnableMongoDbEndpoint”命令行选项手动启动终结点。 现在，它就像所有其他服务终结点（例如 Gremlin、Cassandra 和表）一样。
- 修复了使用“/AllowNetworkAccess”启动时模拟器中的 bug，即 Gremlin、Cassandra 和表终结点无法正确处理外部客户端发出的请求。
- 将直接连接端口添加到“防火墙规则”设置。

### <a name="240"></a>2.4.0

- 修复了当主计算机上存在网络监视应用（如 Pulse Client）时模拟器无法启动的问题。

<!--Update_Description: new articles on local enulator release notes -->
<!--ms.date: 07/29/2019-->