---
title: 如何在 Azure 门户中查看托管标识的服务主体
description: 在 Azure 门户中查看托管标识的服务主体的分步说明。
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 11/29/2018
ms.date: 08/05/2019
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: 265abb655acb6754f712273b3f3bb361679435ce
ms.sourcegitcommit: 461c7b2e798d0c6f1fe9c43043464080fb8e8246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2019
ms.locfileid: "68818683"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>在 Azure 门户中查看托管标识的服务主体

Azure 资源的托管标识在 Azure Active Directory 中为 Azure 服务提供了一个自动托管标识。 此标识可用于通过支持 Azure AD 身份验证的任何服务的身份验证，这样就无需在代码中插入凭据了。 

本文将介绍如何在 Azure 门户中查看托管标识的服务主体。

## <a name="prerequisites"></a>先决条件

- 如果不熟悉 Azure 资源的托管标识，请查阅[概述部分](overview.md)。
- 如果还没有 Azure 帐户，请注册[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。
- 在[虚拟机](/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity)或[应用程序](/app-service/overview-managed-identity#adding-a-system-assigned-identity)上启用系统分配的标识。

## <a name="view-the-service-principal"></a>查看服务主体

该过程演示如何查看启用了系统分配标识的 VM 的服务主体（相同的步骤也适用于应用程序）。

1. 依次单击“Azure Active Directory”、“企业应用程序”   。
2. 在“应用程序类型”下，选择“所有应用程序”   。
3. 在搜索筛选器框中，键入已启用托管标识的 VM 或应用程序的名称，或从显示的列表中选择它。

   ![在门户中查看托管标识服务主体](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>后续步骤

[Azure 资源的托管标识](/active-directory/managed-identities-azure-resources/overview)

