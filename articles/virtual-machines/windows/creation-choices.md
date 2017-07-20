---
title: "在 Azure 中创建 Windows VM 的不同方式 | Azure"
description: "列出使用 Resource Manager 创建 Windows 虚拟机的不同方式。"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 809ba8f4-b54e-43c5-bbe3-8e710c49971f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
origin.date: 03/02/2017
ms.date: 04/27/2017
ms.author: v-dazen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e607f4816d02f60feb17581f18172a200856b605
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# <a name="different-ways-to-create-a-windows-virtual-machine"></a>创建 Windows 虚拟机的不同方式

由于虚拟机适合不同的用户和目的，因此 Azure 可提供创建虚拟机的不同方法。 这意味着需要对虚拟机及其创建方法做出选择。 本文提供了选择摘要和说明链接。

## <a name="azure-portal"></a>Azure 门户
使用 Azure 门户是一种试用虚拟机的简便方式，尤其是在刚开始摸索 Azure 时。 

[使用门户创建运行 Windows 的虚拟机](../virtual-machines-windows-hero-tutorial.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="template"></a>模板
虚拟机需要资源的组合（如可用性集和存储账户）。 无需单独部署和管理每个资源，可以创建 Azure Resource Manager 模板，用于在单个协调操作中部署和预配所有资源。

* [使用 Resource Manager 模板创建 Windows 虚拟机](ps-template.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="azure-powershell"></a>Azure PowerShell
如果喜欢在命令外壳中工作，可以使用 Azure PowerShell。

* [使用 PowerShell 创建 Windows VM](../virtual-machines-windows-ps-create.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="visual-studio"></a>Visual Studio
通过 Azure Tools for Visual Studio 和 Azure SDK，使用 Visual Studio 构建、管理和部署 VM。

[Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)
