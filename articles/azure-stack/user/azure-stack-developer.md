---
title: 开发适用于 Azure Stack 的应用 | Azure
description: 在 Azure Stack 中制作应用程序原型的开发注意事项
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
origin.date: 08/15/2018
ms.date: 08/27/2018
ms.author: v-junlch
ms.reviewer: ''
ms.openlocfilehash: ebe3a07343ee982515e42eb95a33b1a38cec2675
ms.sourcegitcommit: 9dda276bc6675d7da3070ea6145079f1538588ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42869383"
---
# <a name="develop-for-azure-stack"></a>为 Azure Stack 进行开发

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

即使没有 Azure Stack 环境的访问权限，也可以立即开始开发应用程序。 由于 Azure Stack 交付在数据中心运行的 Azure 服务，因此可以使用类似工具和流程针对 Azure Stack 进行开发，就如同为 Azure 进行开发一样。 通过一些准备工作并使用以下主题中的指导，可以使用 Azure 来模拟 Azure Stack 环境。

* 在 Azure 中，可以创建可部署到 Azure Stack 的 Azure 资源管理器模板。 请参阅[模板注意事项](azure-stack-develop-templates.md)，获取有关开发模板的指导，以确保可移植性。
* Azure 与 Azure Stack 之间的服务可用性和服务版本控制存在差异。 可以使用 [Azure Stack 策略模块](azure-stack-policy-module.md)来限制 Azure Stack 可用的 Azure 服务可用性和资源类型。 约束服务可确保应用程序依赖可用于 Azure Stack 的服务。
* [Azure Stack 快速入门模板](https://github.com/Azure/AzureStack-QuickStart-Templates)是常见的方案示例，演示了如何开发可部署到 Azure 和 Azure Stack 的模板。

<!-- Update_Description: wording update -->