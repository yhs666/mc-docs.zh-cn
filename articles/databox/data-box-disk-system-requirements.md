---
title: Azure Data Box 磁盘系统要求 | Microsoft Docs
description: 了解 Azure Data Box 磁盘的软件和网络要求
services: databox
author: WenJason
ms.service: databox
ms.subservice: disk
ms.topic: article
origin.date: 09/04/2019
ms.date: 12/09/2019
ms.author: v-jay
ms.localizationpriority: high
ms.openlocfilehash: e708fcbe569c0051122b538767cd6f18e07f5378
ms.sourcegitcommit: 8c3bae15a8a5bb621300d81adb34ef08532fe739
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883987"
---
::: zone target="docs"

# <a name="azure-data-box-disk-system-requirements"></a>Azure Data Box Disk 系统要求

本文介绍了 Azure Data Box 磁盘解决方案以及连接到 Data Box 磁盘的客户端的重要系统要求。 建议在部署 Data Box 磁盘之前仔细查看信息，然后在部署和后续操作期间根据需要重新参考。

系统要求包括连接到磁盘的客户端支持的平台、支持的存储帐户和存储类型。

::: zone-end

::: zone target="chromeless"

## <a name="review-prerequisites"></a>查看先决条件

1. 你一定已使用[教程：订购 Azure Data Box Disk](data-box-disk-deploy-ordered.md) 订购了 Data Box Disk。 你已收到磁盘，每个磁盘都连接一根电缆。
2. 你有一台可用的客户端计算机，可以从中复制数据。 客户端计算机必须：

    - 运行受支持的操作系统。
    - 安装其他必需的软件。

::: zone-end

## <a name="supported-operating-systems-for-clients"></a>客户端支持的操作系统

下面是通过连接到 Data Box 磁盘的客户端进行磁盘解锁和数据复制操作所支持的操作系统列表。

| **操作系统** | **测试的版本** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows（64 位） |7, 8, 10 |
|Linux <br> <li> Ubuntu </li><li> Debian </li><li> Red Hat Enterprise Linux (RHEL) </li><li> CentOS| <br>14.04、16.04、18.04 <br> 8.11、9 <br> 7.0 <br> 6.5、6.9、7.0、7.5 |  

## <a name="other-required-software-for-windows-clients"></a>Windows 客户端所需的其他软件

对于 Windows 客户端，还应安装以下软件。

| **软件**| **版本** |
| --- | --- |
| Windows PowerShell |5.0 |
| .NET framework |4.5.1 |
| Windows Management Framework |5.0|
| BitLocker| - |

## <a name="other-required-software-for-linux-clients"></a>Linux 客户端所需的其他软件

对于 Linux 客户端，Data Box Disk 工具集安装以下必需软件：

- dislocker
- OpenSSL

## <a name="supported-connection"></a>支持的连接

包含数据的客户端计算机必须拥有 USB 3.0 或更高版本的端口。 磁盘使用提供的数据线连接到此客户端。

## <a name="supported-storage-accounts"></a>支持的存储帐户

下面是 Data Box 磁盘支持的存储类型列表。

| **存储帐户** | **说明** |
| --- | --- |
| 经典 | 标准 |
| 常规用途  |标准；同时支持 V1 和 V2。 同时支持热层和冷层。 |
| Blob 存储帐户 | |

>[!NOTE]
> 不支持 Azure Data Lake Storage Gen 2 帐户。


## <a name="supported-storage-types-for-upload"></a>支持的用于上传的存储类型

下面是一个列表，其中的存储类型可以使用 Data Box Disk 上传到 Azure。

| **文件格式** | **说明** |
| --- | --- |
| Azure 块 blob | |
| Azure 页 blob  | |
| Azure 文件  | |
| 托管磁盘 | |

::: zone target="docs"

## <a name="next-step"></a>后续步骤

* [部署 Azure Data Box 磁盘](data-box-disk-deploy-ordered.md)

::: zone-end

