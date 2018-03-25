---
title: Azure Active Directory PoC 演练手册简介 | Microsoft Docs
description: 浏览并快速实现标识和访问管理方案
services: active-directory
keywords: Azure Active Directory, 操作手册, 概念证明, PoC
documentationcenter: ''
author: dstefanMSFT
manager: asuthar
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: v-junlch
ms.openlocfilehash: ff8e95b438e6a9ccb588a9ee667e45679d651658
ms.sourcegitcommit: ba39acbdf4f7c9829d1b0595f4f7abbedaa7de7d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2018
---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a>Azure Active Directory 概念证明演练手册：简介

本文提供了以概念证明 (PoC) 方式探索不同 Azure AD 功能的指南。 本文档面向的受众是标识架构师、IT 专业人员和系统集成商

## <a name="how-to-use-this-playbook"></a>如何使用此演练手册

1. 使用[主题](active-directory-playbook-ingredients.md#theme)部分，根据需要选取感兴趣的领域。  
2. 选择符合业务目标的方案，为 PoC 设定一个范围。 方案越短越好。 我们建议，方案应尽可能短小、简洁，为利益干系人带来价值的同时，尽量降低其实现过程的复杂性。  
3. 通过[实现](active-directory-playbook-implementation.md)部分了解方案，以及这些方案对环境意味着什么。 在每个方案中，我们会介绍如何建立该方案（涉及[构建基块](active-directory-playbook-building-blocks.md)），以及如何导航这些方案。 
4. 每个构建基块都会说明所需的先决条件，以及大致的完成时间。 这在规划过程中会有所帮助。 
5. 根据上面的步骤 1-3，定义执行操作所需的[环境](active-directory-playbook-ingredients.md#environment)。 建议努力定义一个生产环境，为用户提供良好体验。 
6. 出现相互冲突的要求时，请根据以下标准进行权衡 
   * 在显示值时以主题为中心  
   * 易于准备、设置和执行方案 
   * 尽量减少执行目标方案的时间 
   * 在约束条件下尽量接近生产环境 

>[!NOTE]
> 为方便起见，整篇文章中会不时提及一些特定的第三方应用程序和产品，作为示例。 Azure AD 支持[应用程序库](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps)中提供的数千种应用程序，可以根据需要和环境使用它们。 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
