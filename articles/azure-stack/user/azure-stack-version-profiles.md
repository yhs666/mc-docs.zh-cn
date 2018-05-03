---
title: 管理 Azure Stack 中的 API 版本配置文件 | Microsoft Docs
description: 了解 Azure Stack 中的 API 版本配置文件。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 8A336052-8520-41D2-AF6F-0CCE23F727B4
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/27/2018
ms.date: 04/23/2018
ms.author: v-junlch
ms.reviewer: sijuman
ms.openlocfilehash: 5fd0acee91166472fb1bb90b1c4e080961597a16
ms.sourcegitcommit: 85828a2cbfdb58d3ce05c6ef0bc4a24faf4d247b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2018
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>管理 Azure Stack 中的 API 版本配置文件

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

API 配置文件指定 Azure 资源提供程序和 Azure REST 终结点的 API 版本。 可以使用 API 配置文件以不同的语言创建自定义客户端。 每个客户端使用 API 配置文件来与 Azure Stack 的适当资源提供程序和 API 版本通信。 

可以创建一个应用来与 Azure 资源提供程序配合运行，而无需明确区分与 Azure Stack 兼容的每个资源提供程序 API 的版本。 只需将应用程序对应到配置文件，SDK 就能还原到适当的 API 版本。


本主题帮助读者了解：
 - Azure Stack 的 API 配置文件。
 - 如何使用 API 配置文件来开发解决方案。
 - 在何处找到代码特定的指导。

## <a name="summary-of-api-profiles"></a>API 配置文件的摘要

- API 配置文件用于表示一组 Azure 资源提供程序及其 API 版本。
- API 配置文件是为了让开发人员创建跨多个 Azure 云的模板而创建的。 API 配置文件旨在满足接口兼容且稳定的需求。
- 配置文件每年发布四次。
- 配置文件的三项命名约定如下：
    - **latest**  
        Azure 中发布的最新 API 版本。
    - **yyyy-mm-dd-hybrid**  
    每两年发布一次，此版本注重于跨多个云的一致性和稳定性。
    - **yyyy-mm-dd-profile**  
    介于最佳稳定性和最新功能之间。

## <a name="azure-resource-manager-api-profiles"></a>Azure 资源管理器 API 配置文件

Azure Stack 不使用全球 Azure 中提供的最新版 API。 在创建自己的解决方案时，需要在 Azure 中找到与 Azure Stack 兼容的每个资源提供程序的 API 版本。

无需研究 Azure Stack 支持的每个资源提供程序和特定版本，而可以直接使用 API 配置文件。 配置文件会指定一组资源提供程序和 API 版本。 SDK 或者使用 SDK 生成的工具将还原到配置文件中指定的目标 api-version。 使用 API 配置文件可以指定应用到整个模板的配置文件版本，在运行时，Azure 资源管理器会选择正确的资源版本。

API 配置文件可与使用 Azure 资源管理器的工具（例如 PowerShell、Azure CLI、SDK 中提供的代码，以及 Microsoft Visual Studio）配合运行。 工具和 SDK 可以使用配置文件来读取生成应用程序时要包含的模块和库的版本。

例如，如果使用 PowerShell 以支持 api-version 2016-03-30 的 **Microsoft.Storage** 资源提供程序创建存储帐户，并使用 api-version 为 2015-12-01 的 Microsoft.Compute 资源提供程序创建 VM，则需要查看哪个 PowerShell 模块支持将 2016-03-30 用于存储，以及哪个模块支持将 2015-02-01 用于计算，然后安装这两个模块。 可以改用配置文件。 使用 cmdlet **Install-Profile *profilename***，然后 PowerShell 会加载正确的模块版本。

同样，在使用 Python SDK 生成基于 Python 的应用程序时，可以指定配置文件。 SDK 将为脚本中指定的资源提供程序加载正确的模块。

开发人员可以专注于编写解决方案。 无需研究哪个 api-version、资源提供程序和云可一起运行，而可以使用配置文件，确定代码可跨所有支持该配置文件的云运行。

## <a name="api-profile-code-samples"></a>API 配置文件代码示例

可以借助代码示例，通过配置文件将采用偏好语言的解决方案与 Azure Stack 相集成。 目前可以找到以下语言的指导和示例：

- **PowerShell**  
可以使用通过 PowerShell 库提供的 **AzureRM.Bootstrapper** 模块来获取使用 API 版本配置文件所需的 PowerShell cmdlet。  
有关信息，请参阅[使用适用于 PowerShell 的 API 版本配置文件](azure-stack-version-profiles-powershell.md)。
- **Azure CLI 2.0**  
可将环境配置更新为使用 Azure Stack 特定的 API 版本配置文件。  
有关信息，请参阅[使用适用于 Azure CLI 2.0 的 API 版本配置文件](azure-stack-version-profiles-azurecli2.md)。
- **GO**  
在 GO SDK 中，配置文件结合了不同服务的不同版本的不同资源类型。 配置文件在 profiles/ 路径下提供，其版本采用 **YYYY-MM-DD** 格式。  
有关信息，请参阅[使用适用于 GO 的 API 版本配置文件](azure-stack-version-profiles-go.md)。

## <a name="next-steps"></a>后续步骤
- [安装适用于 Azure Stack 的 PowerShell](azure-stack-powershell-install.md)
- [配置 Azure Stack 用户的 PowerShell 环境](azure-stack-powershell-configure-user.md)
- [查看配置文件支持的资源提供程序 API 版本的详细信息](azure-stack-profiles-azure-resource-manager-versions.md)。

<!-- Update_Description: wording update -->