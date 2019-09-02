---
title: 常用条件访问策略 - Azure Active Directory
description: 组织常用的条件访问策略
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
origin.date: 08/16/2019
ms.date: 08/20/2019
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5d8fab9b4212cee0527c81f267f9da58c11b8f1
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993660"
---
# <a name="common-conditional-access-policies"></a>常用条件访问策略

基线保护策略很好，但是许多组织需要比这些策略提供的更多的灵活性。 例如，许多组织都需要能够从需要多重身份验证的条件访问策略中排除特定帐户，如其紧急访问帐户或不受限管理帐户。 对于这些组织，可以使用本文中提到的常用策略。

![Azure 门户中的条件访问策略](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>紧急访问帐户

有关紧急访问帐户及其重要原因的详细信息，请参阅以下文章： 

* [在 Azure AD 中管理紧急访问帐户](../users-groups-roles/directory-emergency-access.md)

## <a name="typical-policies-deployed-by-organizations"></a>组织部署的典型策略

* [要求对管理员执行 MFA](howto-conditional-access-policy-admin-mfa.md)
* [要求将 MFA 用于 Azure 管理](howto-conditional-access-policy-azure-management.md)
* [阻止旧式身份验证](howto-conditional-access-policy-block-legacy.md)
* [基于风险的条件访问（需要 Azure AD Premium P2）](howto-conditional-access-policy-risk.md)
* [按位置阻止访问](howto-conditional-access-policy-location.md)
* [需要兼容设备](howto-conditional-access-policy-compliant-device.md)

## <a name="next-steps"></a>后续步骤

[使用条件访问 What If 工具模拟登录行为](troubleshoot-conditional-access-what-if.md)

