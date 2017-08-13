---
title: "在 Azure 的 Windows VM 上部署应用程序框架 | Azure"
description: "使用 Azure Resource Manager 模板在 Windows VM 上创建常用应用程序框架，以便安装 Active Directory、Docker，等等。"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 67a67141-b095-44ff-bfdf-7311d1c28b89
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 05/19/2017
ms.date: 10/25/2016
ms.author: v-dazen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3d6d40ffb63ab8abe99d52935ea1b809d2b3ddff
ms.sourcegitcommit: f858adac6a7a32df67bcd5c43946bba5b8ec6afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2017
---
# <a name="deploy-popular-application-frameworks-on-windows-using-azure-resource-manager-templates"></a>使用 Azure Resource Manager 模板在 Windows 上部署常用的应用程序框架 

工作负荷通常要求多项资源按设计运行。 利用 Azure Resource Manager 模板，不仅可以定义应用程序的配置方式，而且可以定义资源的部署方式，以便为配置的应用程序提供支持。 本文介绍库中最常用的模板以及如何使用 Azure 门户、Azure CLI 或 PowerShell 部署它们。

[!INCLUDE [virtual-machines-common-app-frameworks](../../../includes/virtual-machines-common-app-frameworks.md)]
