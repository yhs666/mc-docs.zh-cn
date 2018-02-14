---
title: "排查 Windows VM 分配失败 | Azure"
description: "排查在 Azure 中创建、重启 Windows VM 或调整其大小时发生的分配失败"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: rockboyfor
manager: digimobile
editor: 
tags: top-support-issue,azure-resource-manager,azure-service-management
ms.assetid: bb939e23-77fc-4948-96f7-5037761c30e8
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
origin.date: 11/03/2016
ms.date: 02/05/2018
ms.author: v-yeche
ms.openlocfilehash: d5861e315bf7ff4b44d033b6e61179b17dd51ad6
ms.sourcegitcommit: 3629fd4a81f66a7d87a4daa00471042d1f79c8bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2018
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-windows-vms-in-azure"></a>排查在 Azure 中创建、重启 Windows VM 或调整其大小时发生的分配失败
创建 VM、重新启动已停止（解除分配）的 VM 和重设 VM 大小时，Azure 会为订阅分配计算资源。 执行这些操作时，即使尚未达到 Azure 订阅限制，也可能偶尔收到错误。 本文说明一些常见分配故障的原因，并建议可能的补救方法。 计划服务的部署时，本信息可能也有用。 还可以[排查在 Azure 中创建、重新启动 Linux VM 或调整其大小时发生的分配失败](../linux/allocation-failure.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)。

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]
<!--Update_Description: update meta properties-->