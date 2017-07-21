---
title: "在 AAD 中为 Windows 创建工作或学校标识 | Azure"
description: "了解如何在 Azure Active Directory 中创建工作或学校标识以用于 Windows 虚拟机。"
services: virtual-machines-windows
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d07dca34-618a-48aa-9971-03d9c1210f4a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 08/23/2016
ms.date: 10/25/2016
ms.author: v-dazen
ms.openlocfilehash: ebec0131b39b3f37f2279e9bd710a8e01fda4e36
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-windows-vms"></a>在 Azure Active Directory 中创建工作或学校标识以用于 Windows VM
如果创建了个人 Azure 帐户 - 则表示已使用 Azure.cn 帐户标识创建它。 Azure 的许多强大功能 - [资源组模板](../../azure-resource-manager/resource-group-overview.md)就是一个例子 - 需要有工作或学校帐户（由 Azure Active Directory 管理的标识）才能运行。 可以遵循以下说明创建新的工作帐户或学校帐户，因为在个人 Azure 帐户方面，最有利的特点之一是，这种帐户附带了一个默认 Azure Active Directory 域，使用该域可以创建新的工作或学校帐户用于需要这种帐户的 Azure 功能。

但是，利用最近所做的更改，可以使用[此处](../../xplat-cli-connect.md)所述的 `azure login -e AzureChinaCloud` 交互式登录方法，通过任何类型的 Azure 帐户管理订阅。 可以使用该机制，也可以遵循后面的说明。 还可以[在 Azure Active Directory 中创建工作或学校标识以用于 Linux VM](../linux/create-aad-work-id.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)。

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

[!INCLUDE [virtual-machines-common-create-aad-work-id](../../../includes/virtual-machines-common-create-aad-work-id.md)]
