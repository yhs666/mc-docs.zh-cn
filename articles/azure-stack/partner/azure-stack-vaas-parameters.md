---
title: Azure Stack 验证即服务中的工作流常用参数 | Azure
description: Azure Stack 验证即服务的工作流常用参数
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/24/2018
ms.date: 08/27/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: c40712eb2adf6bde668aa3196325c3848996fa91
ms.sourcegitcommit: 9dda276bc6675d7da3070ea6145079f1538588ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42869692"
---
# <a name="workflow-common-parameters-for-azure-stack-validation-as-a-service"></a>Azure Stack 验证即服务的工作流常用参数

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

常用参数包括验证即服务 (VaaS) 中的所有测试必需的环境变量和用户凭据等值。 这些值是在工作流级别定义的。 创建或修改工作流时将保存这些值。 在调度时，工作流将为测试加载这些值。 

## <a name="environment-parameters"></a>环境参数

环境参数描述待测试的 Azure Stack 环境。 必须通过生成并上传戳记配置文件 `&lt;link&gt;. [How to get the stamp info link].` 来提供这些值

| 参数名称 | 必需 | 类型 | 说明 |
|----------------------------------|----------|------|---------------------------------------------------------------------------------------------------------------------------------|
| Azure Stack 内部版本 | 必需 |  | Azure Stack 部署的内部版本号（例如 1.0.170330.9） |
| OEM 版本 | 是 |  | 在部署 Azure Stack 期间使用的 OEM 程序包的版本号。 |
| OEM 签名 | 是 |  | 在部署 Azure Stack 期间使用的 OEM 程序包的签名。 |
| AAD 租户 ID | 必需 |  | 在部署 Azure Stack 期间指定的 Azure Active Directory 租户 GUID。|
| 区域 | 必需 |  | Azure Stack 部署区域。 |
| 租户资源管理器终结点 | 必需 |  | 租户 Azure 资源管理器操作的终结点（例如 https://management。<ExternalFqdn>） |
| 管理员资源管理器终结点 | 是 |  | 租户 Azure 资源管理器操作的终结点（例如 https://adminmanagement。<ExternalFqdn>） |
| 外部 FQDN | 是 |  | 用作终结点后缀的外部完全限定的域名。 （例如 local.azurestack.external 或 redmond.contoso.com）。 |
| 节点数 | 是 |  | 部署中的节点数。 |

## <a name="test-parameters"></a>测试参数

常用测试参数包括不能在配置文件中存储的敏感信息，必须手动提供。

| 参数名称 | 必需 | 类型 | 说明 |
|--------------------------------|------------------------------------------------------------------------------|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 租户用户名 | 必需 |  | 之前已预配的或者需要由 AAD Directory 中的服务管理员预配的 Active Directory 租户管理员。 测试使用此值来执行租户级操作，例如，部署模板来预配资源（VM、存储帐户，等等）并执行工作负荷。 测试使用此值来执行租户级操作，例如，部署模板来预配资源（VM、存储帐户，等等）并执行工作负荷。 |
| 租户密码 | 必需 |  | 租户用户的密码。 |
| 服务管理员用户名 | 必需：解决方案验证、程序包验证<br>非必需：测试轮次 |  | 在部署 Azure Stack 期间指定的 AAD Directory 租户的 Azure Active Directory 管理员。 |
| 服务管理员密码 | 必需：解决方案验证、程序包验证<br>非必需：测试轮次 |  | 服务管理员用户的密码。 |
| 云管理员用户名 | 必需 |  | Azure Stack 域管理员帐户（例如 contoso\cloudadmin）。 在配置文件中搜索 User Role="CloudAdmin" 并选择配置文件中 UserName 标记中的值。 |
| 云管理员密码 | 必需 |  | 云管理员用户的密码。 |
| 诊断连接字符串 | 必需 |  | 在执行测试期间要将诊断日志复制到的 Azure 存储帐户的 SAS URI。 [设置 blob 存储帐户](azure-stack-vaas-set-up-account.md)中提供了有关生成 SAS URI 的说明。 |


## <a name="next-steps"></a>后续步骤

- 详细了解 [Azure Stack 验证即服务](/azure/azure-stack/partner)。
