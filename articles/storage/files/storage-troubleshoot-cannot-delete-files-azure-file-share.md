---
title: Azure 文件共享 - 无法从 Azure 文件共享中删除文件
description: 确定并排查无法从 Azure 文件共享中删除文件的问题。
author: WenJason
ms.topic: troubleshooting
ms.author: v-jay
origin.date: 10/25/2019
ms.date: 11/25/2019
ms.service: storage
ms.subservice: common
ms.openlocfilehash: a01945f4521169197554579fc230e71d8a8cf7d8
ms.sourcegitcommit: 6a19227dcc0c6e0da5b82c4f69d0227bf38a514a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74328780"
---
# <a name="azure-file-share--failed-to-delete-files-from-azure-file-share"></a>Azure 文件共享 - 无法从 Azure 文件共享中删除文件

无法从 Azure 文件共享中删除文件可能有几种症状：

**症状 1：**

由于以下两个问题之一，无法删除 Azure 文件共享中的文件：

* 标记为删除的文件
* SMB 客户端可能正在使用指定的资源

**症状 2：**

处理此命令的配额不够

## <a name="cause"></a>原因

在要装载文件共享的计算机上，如果达到文件允许的并发打开句柄上限，便会出现错误 1816。 有关详细信息，请参阅 [Azure 存储性能和可伸缩性清单](/storage/blobs/storage-performance-checklist)。

## <a name="resolution"></a>解决方法

请关闭一些句柄，减少并发打开句柄的数量。

## <a name="prerequisite"></a>先决条件

### <a name="install-the-latest-azure-powershell-module"></a>安装最新的 Azure PowerShell 模块

* [安装 Azure Powershell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)

### <a name="connect-to-azure"></a>连接到 Azure：

```
# Connect-AzAccount -Environment AzureChinaCloud
```

### <a name="select-the-subscription-of-the-target-storage-account"></a>选择目标存储帐户的订阅：

```
# Select-AzSubscription -subscriptionid "SubscriptionID"
```

### <a name="create-context-for-the-target-storage-account"></a>为目标存储帐户创建上下文：

```
$Context = New-AzStorageContext -StorageAccountName "StorageAccountName" -StorageAccountKey "StorageAccessKey"
```

### <a name="get-the-current-open-handles-of-the-file-share"></a>获取文件共享的当前打开句柄：

```
# Get-AzStorageFileHandle -Context $Context -ShareName "FileShareName" -Recursive
```

## <a name="example-result"></a>示例结果：

|HandleId|`Path`|ClientIp|ClientPort|OpenTime|LastReconnectTime|FileId|ParentId|SessionId|
|---|---|---|---|---|---|---|---|---|
|259101229083|---|10.222.10.123|62758|2019-10-05|12:16:50Z|0|0|9507758546259807489|
|259101229131|---|10.222.10.123|62758|2019-10-05|12:36:20Z|0|0|9507758546259807489|
|259101229137|---|10.222.10.123|62758|2019-10-05|12:36:53Z|0|0|9507758546259807489|
|259101229136|New folder/test.zip|10.222.10.123|62758|2019-10-05|12:36:29Z|13835132822072852480|9223446803645464576|9507758546259807489|
|259101229135|test.zip|37.222.22.143|62758|2019-10-05|12:36:24Z|11529250230440558592|0|9507758546259807489|

### <a name="close-an-open-handle"></a>关闭打开的句柄：

若要关闭打开的句柄，请使用以下命令：

```
# Close-AzStorageFileHandle -Context $Context -ShareName "FileShareName" -Path 'New folder/test.zip' -CloseAll
```

## <a name="next-steps"></a>后续步骤

* [在 Windows 中排查 Azure 文件问题](storage-troubleshoot-windows-file-connection-problems.md)
* [在 Linux 中排查 Azure 文件问题](storage-troubleshoot-linux-file-connection-problems.md)
