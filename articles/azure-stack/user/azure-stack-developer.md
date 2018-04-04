---
title: 开发适用于 Azure Stack 的应用 | Microsoft Docs
description: 了解在 Azure Stack 中制作应用程序原型的开发注意事项
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 09/25/2017
ms.date: 03/08/2018
ms.author: v-junlch
ms.reviewer: ''
ms.openlocfilehash: a0a464fe38c25623ec819eabee5f9559fa234be6
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="develop-for-azure-stack"></a>为 Azure Stack 进行开发
即使没有 Azure Stack 环境的访问权限，也可以立即开始开发应用程序。 由于 Azure Stack 交付在数据中心运行的 Azure 服务，因此可以使用类似工具和流程针对 Azure Stack 进行开发，就如同为 Azure 进行开发一样。  只需遵循以下主题中所述的准备工作和指导，就能使用 Azure 来模拟 Azure Stack 环境：

- 在 Azure 中，可以创建 Azure 资源管理器模板，它们也可以部署到 Azure Stack。  请参阅[模板注意事项](azure-stack-develop-templates.md)，获取有关开发模板的指导，以确保可移植性。
- Azure 与 Azure Stack 之间的服务可用性和服务版本存在差异。 可以使用 [Azure Stack 策略模块](azure-stack-policy-module.md)来限制 Azure Stack 可用的 Azure 服务可用性和资源类型。 约束可用服务可帮助应用程序依赖于 Azure Stack 可用的服务。
- [Azure Stack 快速入门模板](https://github.com/Azure/AzureStack-QuickStart-Templates)是演示如何开发模板，使其能够部署到 Azure 和 Azure Stack 的常见方案示例。



