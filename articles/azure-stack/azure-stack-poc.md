---
title: 什么是 Azure Stack？ | Microsoft Docs
description: Azure Stack 允许在数据中心内运行 Azure 服务。
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
origin.date: 03/22/2018
ms.date: 05/24/2018
ms.author: v-junlch
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: 32819ff712924511cb8f72ffe9165cfdd26c6785
ms.sourcegitcommit: 036cf9a41a8a55b6f778f927979faa7665f4f15b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "34475007"
---
# <a name="what-is-azure-stack"></a>什么是 Azure Stack？

Azure Stack 是一种混合云平台，通过它可从组织的数据中心提供 Azure 服务。  Azure Stack 旨在为重要场景（如边缘环境和离线环境，或者满足特定安全性和合规性要求）中的现代应用程序启用新方案。  Azure Stack 以两个部署选项提供，以满足你的需求。

## <a name="azure-stack-integrated-systems"></a>Azure Stack 集成系统
Azure Stack 集成系统通过 Microsoft 和[硬件合作伙伴](https://azure.microsoft.com/overview/azure-stack/integrated-systems/)的合作关系提供，它创建的解决方案兼顾云时代的创新与管理的简化。  由于 Azure Stack 作为硬件和软件的集成系统提供，因此它不但向你提供适量的灵活性和控制，而且还采用云环境带来的创新。  Azure Stack 集成系统的大小范围从 4 个节点到 12 个节点，由硬件合作伙伴和 Microsoft 共同提供支持。  使用 Azure Stack 集成系统可为生产工作负荷启用新方案。    

## <a name="azure-stack-development-kit"></a>Azure Stack 开发工具包
[Azure Stack 开发工具包 (ASDK)](asdk/asdk-what-is.md) 是单节点部署的 Azure Stack，可用于评估和了解 Azure Stack。  还可以使用 ASDK 作为开发人员环境，可在其中使用与 Azure 一致的 API 和工具进行开发。 不应将 ASDK 用作生产环境。

ASDK 具有以下限制：
- ASDK 与单个 Azure Active Directory (Azure AD) 或 Active Directory 联合身份验证服务 (AD FS) 标识提供者相关联。 可在此目录中创建多个用户，并将订阅分配给每个用户。
- 如果在单个计算机上部署所有组件，租户资源可用的物理资源可能有限。 此配置不适用于规模或性能评估。
- 网络方案根据单一主机/NIC 的要求受到限制。  

## <a name="next-steps"></a>后续步骤
[主要功能和概念](azure-stack-key-features.md)

[Azure Stack：Azure 的扩展 (pdf)](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)


<!-- Update_Description: link update -->