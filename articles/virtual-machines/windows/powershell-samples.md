---
title: Azure 虚拟机 PowerShell 示例 | Azure
description: Azure 虚拟机 PowerShell 示例
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 11/30/2017
ms.date: 06/04/2018
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 165230abf6f053e4ac3661a729344fc93518996a
ms.sourcegitcommit: bdffde936fa2a43ea1b5b452b56d307647b5d373
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42871610"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Azure 虚拟机 PowerShell 示例

下表包含用于创建和管理 Windows 虚拟机的 PowerShell 脚本示例的链接。

| | |
|---|---|
|**创建虚拟机**||
| [快速创建虚拟机](./../scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 创建资源组、虚拟机以及所有相关资源，并且出现的提示最少。|
| [创建完全配置的虚拟机](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 创建资源组、虚拟机以及所有相关资源。|
| [创建高度可用的虚拟机](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 使用高度可用且负载均衡的配置创建多个虚拟机。|
| [创建 VM 并运行配置脚本](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 创建一个虚拟机，并使用 Azure 自定义脚本扩展安装 IIS。 |
| [创建 VM 并运行 DSC 配置](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 创建一个虚拟机，并使用 Azure Desired State Configuration (DSC) 扩展来安装 IIS。 |
| [上传 VHD 并创建 VM](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | 将本地 VHD 文件上传到 Azure，从 VHD 创建映像，然后从该映像创建 VM。 |
| [从托管 OS 磁盘创建 VM](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 通过将现有托管磁盘附加为 OS 磁盘来创建虚拟机。 |
| [从快照创建 VM](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 先从快照创建托管磁盘，然后将新的托管磁盘附加为 OS 磁盘来从快照创建虚拟机。 |
|**管理存储**||
| [从相同或不同订阅中的 VHD 创建托管磁盘](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 从相同或不同订阅中作为 OS 磁盘的专用 VHD 或作为数据磁盘的数据 VHD 创建托管磁盘。  |
| [从快照创建托管磁盘](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 从快照创建托管磁盘。 |
| [将托管磁盘复制到相同或不同的订阅](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | 将托管磁盘复制到相同或不同的订阅，但与父级托管磁盘位于同一区域。 
| [将快照作为 VHD 导出到存储帐户](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 将托管快照作为 VHD 导出到不同区域中的存储帐户。 |
| [从 VHD 创建快照](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 从 VHD 创建快照以在短时间内从快照创建多个相同的托管磁盘。  |
| [将快照复制到相同或不同订阅](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 将快照复制到相同或不同的订阅，但与父级快照位于同一区域。 |
|**保护虚拟机安全**||
| [加密 VM 和数据磁盘](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | 创建 Azure Key Vault、加密密钥和服务主体，然后对 VM 进行加密。 |
| | |

<!-- Not Available on Line 45  |**Monitor virtual machines**||-->

<!--Update_Description: update meta properties -->