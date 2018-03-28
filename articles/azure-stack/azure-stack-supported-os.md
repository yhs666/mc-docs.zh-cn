---
title: Azure Stack 支持的来宾操作系统 | Microsoft Docs
description: 在 Azure Stack 上可以使用这些来宾操作系统。
services: azure-stack
documentationcenter: ''
author: Brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 02/22/2018
ms.date: 03/04/2018
ms.author: v-junlch
ms.reviewer: JeffGoldner
ms.openlocfilehash: 39e957eded1455ac2106eff97efa414bd8050276
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="guest-operating-systems-supported-on-azure-stack"></a>Azure Stack 支持的来宾操作系统

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

## <a name="windows"></a>Windows
Azure Stack 支持下表中列出的 Windows 来宾操作系统：Marketplace 中的映像可供下载到 Azure Stack。 Marketplace 中未提供 Windows 客户端映像。

在部署期间，Azure Stack 会将适当版本的来宾代理注入到映像中。

| 操作系统 | 说明 | 发布者 | OS 类型 | 应用商店 |
| --- | --- | --- | --- | --- | --- |
| Windows Server 2008 R2 SP1 | 64 位 | Microsoft | Windows | 数据中心 |
| Windows Server 2012 | 64 位 | Microsoft | Windows | 数据中心 |
| Windows Server 2012 R2 | 64 位 | Microsoft | Windows | 数据中心 |
| Windows Server 2016 | 64 位 | Microsoft | Windows | 数据中心、数据中心核心、包含容器的数据中心 |
| Windows 10*（请参见注释 1）* | 64 位，Pro 和 Enterprise | Microsoft | Windows | 否 |

***注释 1：****若要在 Azure Stack 上部署 Windows 10 客户端操作系统，必须具备 Windows 每用户授权，或者通过合格多租户托管商 (QMTH) 购买。*


## <a name="linux"></a>Linux

此处列出的 Linux 发行版包括必要的 Azure Linux 代理 (WALA)。

> [!NOTE]   
> *不*支持使用 WALA 2.2.3 以下版本构建的映像，且不太可能进行部署。 某些 WALA 代理版本已知无法在 Azure Stack VM 上正常工作，包括 2.2.12 版和 2.2.13 版。
>
> [cloud-init](https://cloud-init.io/) 仅适用于 Azure Stack 上的 Ubuntu 发行版。

| 分发 | 说明 | 发布者 | 应用商店 |
| --- | --- | --- | --- | --- | --- |
| 容器 Linux |  64 位 | CoreOS | Stable |
| 基于 CentOS 的 6.9 | 64 位 | Rogue Wave | 是 |
| 基于 CentOS 的 7.4 | 64 位 | Rogue Wave | 是 |
| Debian 8 "Jessie" | 64 位 | credativ |  是 |
| Debian 9“Stretch” | 64 位 | credativ | 是 |
| Red Hat Enterprise Linux 7.x（待定） | 64 位 | Red Hat | 否 |
| SLES 11SP4 | 64 位 | SUSE | 是 |
| SLES 12SP3 | 64 位 | SUSE | 是 |
| Ubuntu 14.04-LTS | 64 位 | Canonical | 是 |
| Ubuntu 16.04-LTS | 64 位 | Canonical | 是 |

将来可能支持其他 Linux 发行版。

