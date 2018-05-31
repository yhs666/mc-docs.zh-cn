---
title: 为 Azure 和 Azure Stack 应用程序配置混合云标识 | Microsoft Docs
description: 了解如何为 Azure 和 Azure Stack 应用程序配置混合云标识。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
origin.date: 05/07/2018
ms.date: 05/23/2018
ms.author: v-junlch
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: f84bfded85a0b808a455d7d1f5c71b0ee892a443
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475120"
---
# <a name="tutorial-configure-hybrid-cloud-identity-with-azure-and-azure-stack-applications"></a>教程：为 Azure 和 Azure Stack 应用程序配置混合云标识

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

有两个选项可用来向全球 Azure 和 Azure Stack 中的应用程序授予访问权限。 当 Azure Stack 具有到 Internet 的不间断连接时，可以使用 Azure Active Directory (Azure AD)。 当 Azure Stack 从 Internet 断开了连接时，可以使用 Azure Directory 联合身份验证服务 (AD FS)。 使用服务主体向 Azure Stack 中的应用程序授予访问权限，以便在 Azure Stack 中通过 Azure 资源管理器进行部署或配置。 

在本教程中，将构建一个示例环境来完成以下任务：

> [!div class="checklist"]
> * 在全球 Azure 和 Azure Stack 中建立一个混合标识
> * 检索令牌来访问 Azure Stack API。

执行这些步骤需要你具有 Azure Stack 操作员权限。

## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>在门户中创建适用于 Azure AD 的服务主体

如果已使用 Azure AD 部署 Azure Stack 作为标识存储，则可以创建服务主体，就像对 Azure 所做的那样。 [此文档](/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad)展示了如何通过门户执行这些步骤。 在开始之前，请检查是否具有[所需的 Azure AD 权限](/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions)。

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>使用 PowerShell 创建适用于 AD FS 的服务主体

如果已使用 AD FS 部署 Azure Stack，则可以使用 PowerShell 创建服务主体、为角色分配访问权限以及使用该标识从 PowerShell 登录。 [此文档](/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs)展示了如何使用 PowerShell 执行这些步骤。

## <a name="using-the-azure-stack-api"></a>使用 Azure Stack API

[此教程](/azure-stack/user/azure-stack-rest-api-use)将引导你完成检索令牌来访问 Azure Stack API 的过程。

## <a name="connect-to-azure-stack-using-powershell"></a>使用 Powershell 连接到 Azure Stack

下面是一个示例脚本，它使用了本文档中讲授的有关连接到 Azure Stack 的概念。

### <a name="prerequisites"></a>先决条件

一个通过你可以访问的订阅连接到 Azure Active Directory 的 Azure Stack 安装。 如果没有 Azure Stack 安装，可以使用这些说明来安装 [Azure Stack 开发工具包](/azure-stack/asdk/asdk-deploy)。

[此教程](/azure-stack/azure-stack-powershell-configure-quickstart)将引导你完成安装 Azure PowerShell 以及连接到 Azure Stack 安装所需执行的步骤。

#### <a name="connect-to-azure-stack-using-code"></a>使用代码连接到 Azure Stack

若要使用代码连接到 Azure Stack，请使用 Azure 资源管理器终结点 API 来获取 Azure Stack 安装的身份验证和 Graph 终结点，然后使用 REST 请求进行身份验证。 可以在[此处](https://github.com/shriramnat/HybridARMApplication)找到示例应用程序。

> [!Note]  
除非所选语言的 Azure SDK 支持 Azure API 配置文件，否则 SDK 可能无法与 Azure Stack 一起使用。 若要详细了解 Azure API 配置文件，请转到[此处](/azure-stack/user/azure-stack-version-profiles)。

## <a name="next-steps"></a>后续步骤

 - 若要详细了解 Azure Stack 如何处理标识，请参阅 [Azure Stack 的标识体系结构](/azure-stack/azure-stack-identity-architecture)。  


