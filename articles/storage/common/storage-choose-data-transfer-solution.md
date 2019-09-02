---
title: 选择 Azure 数据传输解决方案 | Microsoft Docs
description: 了解基于环境中的数据大小和可用网络带宽如何选择 Azure 数据传输解决方案
services: storage
author: WenJason
ms.service: storage
ms.subservice: blobs
ms.topic: article
origin.date: 06/03/2019
ms.date: 07/15/2019
ms.author: v-jay
ms.openlocfilehash: 4bed5756bd0be88bf607664a539aaa608fa3f268
ms.sourcegitcommit: 80336a53411d5fce4c25e291e6634fa6bd72695e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844464"
---
# <a name="choose-an-azure-solution-for-data-transfer"></a>选择 Azure 数据传输解决方案

本文概述一些常用 Azure 数据传输解决方案。 本文还根据环境中的网络带宽以及打算传输的数据大小提供了建议选项。

## <a name="types-of-data-movement"></a>数据移动的类型

数据传输可以脱机进行或通过网络连接进行。 根据以下因素选择解决方案：

- **数据大小** - 打算进行传输的数据大小，
- **传输频率** - 一次性或定期数据引入，以及
- **网络** - 环境中可用于数据传输的带宽。

数据移动可以是以下类型：

- **使用可发运设备脱机传输** - 在要进行脱机一次性批量数据传输时使用可发运设备。 Azure 会向你寄送磁盘或安全的专用设备。 或者，你可以购买并发运自己的磁盘。 将数据复制到设备，然后将它发运给在其中上传数据的 Azure。  适用于此示例的选项为“导入/导出”（使用自己的磁盘）。

- **网络传输** - 通过网络连接将数据传输到 Azure。 这可以通过多种方法来实现。

    - **图形界面** - 如果偶尔仅传输几个文件，并且无需自动执行数据传输，则可以选择图形界面工具（如 Azure 存储资源管理器或 Azure 门户中基于 Web 的浏览工具）。
    - **脚本化或编程传输** - 可以使用我们提供的优化软件工具，或直接调用我们的 REST API/SDK。 可用的可编写脚本工具有 AzCopy、Azure PowerShell 和 Azure CLI。 对于编程接口，请使用用于 .NET、Java、Python、Node/JS、C++、Go、PHP 或 Ruby 的 SDK 之一。

## <a name="next-steps"></a>后续步骤

- [获取 Azure 存储资源管理器简介](https://azure.microsoft.com/resources/videos/introduction-to-microsoft-azure-storage-explorer/)。
- [阅读 AzCopy 概述](/storage/common/storage-use-azcopy-v10)。
- [将 Azure PowerShell 与 Azure 存储一起使用](/storage/common/storage-powershell-guide-full)
- [将 Azure CLI 用于 Azure 存储](/storage/common/storage-azure-cli)
- [了解什么是 Azure 数据工厂](https://docs.microsoft.com/azure/data-factory/copy-activity-overview)。
- 使用 REST API 传输数据

    - [在 .NET 中](/dotnet/api/overview/storage)
    - [在 Java 中](https://docs.azure.cn/zh-cn/java/api/storage/clientlibrary)
