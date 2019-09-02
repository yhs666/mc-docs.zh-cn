---
title: 使用 Azure SDK 为 VM 配置 Azure 资源托管标识
description: 分步说明如何使用 Azure SDK 在 Azure VM 上配置和使用 Azure 资源托管标识。
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
origin.date: 09/28/2017
ms.date: 08/05/2019
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5a7a556404f29d69fc81514261f769761090dfbe
ms.sourcegitcommit: 461c7b2e798d0c6f1fe9c43043464080fb8e8246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2019
ms.locfileid: "68818631"
---
# <a name="configure-a-vm-with-managed-identities-for-azure-resources-using-an-azure-sdk"></a>使用 Azure SDK 为 VM 配置具有 Azure 资源托管标识

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure 资源的托管标识在 Azure Active Directory (AD) 中为 Azure 服务提供了一个自动托管标识。 此标识可用于通过支持 Azure AD 身份验证的任何服务的身份验证，这样就无需在代码中插入凭据了。 

本文介绍如何使用 Azure SDK 为 Azure VM 启用和删除 Azure 资源托管标识。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-managed-identities-for-azure-resources-support"></a>Azure SDK 包含 Azure 资源托管标识支持 

Azure 通过一系列 [Azure SDK](/downloads) 支持多个编程平台。 其中多个平台已更新为支持 Azure 资源托管标识，并提供相应的示例来演示用法。 由于添加了其他支持，此列表已更新：

| SDK | 示例 |
| --- | ------ | 
| .NET   | [从具有已启用的 Azure 资源托管标识的已启用的 VM 中管理资源](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [从具有 Azure 资源托管标识的已启用的 VM 中管理存储](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [创建启用了系统分配托管标识的 VM](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [创建启用了系统分配托管标识的 VM](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [创建启用了系统分配托管标识的 Azure VM](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>后续步骤

- 请参阅“为 Azure VM 配置标识”下的相关文章，了解还可以如何使用 Azure 门户、PowerShell、CLI 和资源模板完成此操作  。

