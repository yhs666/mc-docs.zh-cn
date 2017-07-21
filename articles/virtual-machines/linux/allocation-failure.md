---
title: "排查 Linux VM 分配故障 | Azure"
description: "排查在 Azure 中创建、重启 Linux VM 或调整其大小时发生的分配故障"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resourece-manager,azure-service-management
ms.assetid: 1ef41144-6dd6-4a56-b180-9d8b3d05eae7
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
origin.date: 02/02/2016
ms.date: 05/24/2016
ms.author: v-dazen
ms.openlocfilehash: ed7c0b1ec5dd98d5f4d6c638c64805c8687a7df3
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-linux-vms-in-azure"></a>排查在 Azure 中创建、重启 Linux VM 或调整其大小时发生的分配故障
创建 VM、重启已停止（解除分配）的 VM 和重设 VM 大小时，Azure 会为订阅分配计算资源。 执行这些操作时，即使尚未达到 Azure 订阅限制，也可能偶尔收到错误。 本文说明一些常见分配故障的原因，并建议可能的补救方法。 计划服务的部署时，本信息可能也有用。 还可以用于[排查在 Azure 中创建、重启 Windows VM 或调整其大小时发生的分配故障](../windows/allocation-failure.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]
