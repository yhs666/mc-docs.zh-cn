---
title: "在 Azure 的 Linux VM 上部署应用程序框架 | Azure"
description: "使用 Azure Resource Manager 模板在 Linux VM 上创建常用应用程序框架，以便安装 Active Directory、Docker，等等。"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: squillace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 90e95919-4611-40d7-8fa8-e38facbde9a7
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 05/19/2017
ms.date: 10/25/2016
ms.author: v-dazen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9e2673e10e464bb97415d6c5fc4cf1b295d885fe
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# <a name="deploy-popular-application-frameworks-on-linux-using-azure-resource-manager-templates"></a>使用 Azure Resource Manager 模板在 Linux 上部署常用的应用程序框架

工作负荷通常要求多项资源按设计运行。 利用 Azure Resource Manager 模板，不仅可以定义应用程序的配置方式，而且可以定义资源的部署方式，以便为配置的应用程序提供支持。 本文介绍库中最常用的模板以及如何使用 Azure 门户、Azure CLI 或 PowerShell 来部署它们。

[!INCLUDE [virtual-machines-common-app-frameworks](../../../includes/virtual-machines-common-app-frameworks.md)]
