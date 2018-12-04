---
title: 什么是 Azure Stack？ | Microsoft Docs
description: 使用 Azure Stack 可在数据中心内运行 Azure 服务。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
origin.date: 10/15/2018
ms.date: 11/12/2018
ms.author: v-jay
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: 711878e937c3d861f77ddd87876515deac443af3
ms.sourcegitcommit: d75065296d301f0851f93d6175a508bdd9fd7afc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52661760"
---
# <a name="what-is-azure-stack"></a>什么是 Azure Stack？

Azure Stack 是一种混合云平台，用于在数据中心提供 Azure 服务。 此平台旨在支持不断演进的业务要求。 Azure Stack 可为现代应用程序实现新的方案（例如边缘和离线环境），或者满足特定的安全与合规性要求。

Azure Stack 以两个部署选项提供，以满足你的需求。

## <a name="azure-stack-integrated-systems"></a>Azure Stack 集成系统
Azure Stack 集成系统通过 Microsoft 与[硬件合作伙伴](https://azure.microsoft.com/overview/azure-stack/integrated-systems/)的合作关系提供，它创建的解决方案兼顾云时代的创新与计算管理的简化。 由于 Azure Stack 以集成式硬件和软件系统的形式提供，因此你可以获得所需的灵活性和控制度，以及云中的创新能力。 Azure Stack 集成系统的大小范围从 4 个节点到 12 个节点，由硬件合作伙伴和 Microsoft 共同提供支持。  使用 Azure Stack 集成系统可以创建新的方案，以及为生产工作负荷部署新解决方案。

## <a name="azure-stack-development-kit"></a>Azure Stack 开发工具包

Microsoft [Azure Stack 开发工具包 (ASDK)](asdk/asdk-what-is.md) 是 Azure Stack 的单节点部署，可用于评估和了解 Azure Stack。  还可将 ASDK 用作开发人员环境，以使用与 Azure 一致的 API 和工具来构建应用。

>[!Note]
>ASDK 不可用作生产环境。

ASDK 具有以下限制：

* ASDK 与单个 Azure Active Directory (Azure AD) 或 Active Directory 联合身份验证服务 (AD FS) 标识提供者相关联。 可在此目录中创建多个用户，并将订阅分配给每个用户。
* 由于 Azure Stack 组件部署在一台主机上，可用作租户资源的物理资源有限。 此配置不适用于规模或性能评估。
* 由于只有一台主机和 NIC 部署要求方面的原因，网络方案会受到限制。

## <a name="next-steps"></a>后续步骤

[主要功能和概念](azure-stack-key-features.md)

[Azure Stack：Azure 的扩展 (pdf)](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)
<!-- Update_Description: wording update -->