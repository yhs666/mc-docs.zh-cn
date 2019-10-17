---
title: 如何在 Azure Active Directory 标识保护中关闭活动风险检测 | Microsoft Docs
description: 了解用于关闭活动风险检测的选项。
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
origin.date: 09/24/2018
ms.date: 10/10/2019
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f5bf530d2bc81e22b889515d9a801041be26c6c
ms.sourcegitcommit: 74f50c9678e190e2dbb857be530175f25da8905e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2019
ms.locfileid: "72292092"
---
# <a name="how-to-close-active-risk-detections"></a>如何：关闭活动风险检测

Azure Active Directory 使用风险检测来检测可能指示用户帐户已泄露的迹象。 管理员希望能够关闭所有风险检测，以便受影响的用户不再面临风险。

本文概述了用于关闭活动风险检测的附加选项。

## <a name="options-to-close-risk-detections"></a>用于关闭风险检测的选项 

风险检测的状态为“活动”或“已关闭”。   计算称作“用户风险级别”的值时，会考虑到所有活动风险检测。 用户风险级别是反映帐户泄露可能性的一个指标（低、中、高）。 

若要关闭活动风险检测，可使用以下选项：

- 要求使用用户风险策略进行密码重置
- 手动重置密码
- 消除所有风险检测 
- 手动关闭单个风险检测

## <a name="require-password-reset-with-a-user-risk-policy"></a>要求使用用户风险策略进行密码重置

通过配置[用户风险条件访问策略](howto-user-risk-policy.md)，可以要求在自动检测到指定的用户风险级别时更改密码。 

![重置密码](./media/howto-close-active-risk-events/13.png)

密码重置会关闭相关用户的所有活动风险事件，并使标识恢复安全状态。 使用用户风险策略是关闭活动风险检测的首选方法，因为此方法已自动化。 受影响的用户与支持人员或管理员之间不需要交互。

但是，不一定总适合使用用户风险策略。 例如，对于以下情况，用户风险策略就不适用：

- 用户尚未注册多重身份验证 (MFA)。
- 用户的活动风险检测已删除。
- 调查发现合法的用户执行了报告的风险检测。

## <a name="manual-password-reset"></a>手动重置密码

如果无法要求使用用户风险策略重置密码，可以使用手动密码重置关闭用户的所有风险检测。

![重置密码](./media/howto-close-active-risk-events/04.png)

相关对话框中提供了重置密码的两种不同方法：

![重置密码](./media/howto-close-active-risk-events/05.png)

**生成临时密码** - 生成临时密码可以立即让标识恢复安全状态。 此方法需要与受影响的用户交互，因为这些用户需要知道临时密码。 例如，可将新的临时密码发送到用户的备用电子邮件地址或用户的经理。 由于该密码是临时性的，因此系统会在用户下次登录时提示更改密码。

**要求用户重置密码** - 要求用户重置密码可以实现自助恢复，而无需联系支持人员或管理员。 与用户风险策略一样，此方法仅适用于已注册 MFA 的用户。 对于尚未注册 MFA 的用户，此选项不可用。

## <a name="dismiss-all-risk-detections"></a>消除所有风险检测

如果密码重置不可行，也可以消除所有风险检测。 

![重置密码](./media/howto-close-active-risk-events/03.png)

单击“清除所有事件”时，所有事件都会关闭，受影响的用户不再有风险。  但是，此方法不会对现有密码产生影响，因为它不能将相关的标识恢复安全状态。 使用此方法时，首选删除存在活动风险检测的用户。 

## <a name="close-individual-risk-detections-manually"></a>手动关闭单个风险检测

可以手动关闭单个风险检测。 手动关闭风险检测可以降低用户风险级别。 通常，在响应相关调查时需手动关闭风险检测。 例如，在与用户沟通时发现不再需要某个活动风险检测。 
 
手动关闭风险检测时，可以选择执行以下任一操作来更改风险检测的状态：

![操作](./media/howto-close-active-risk-events/06.png)

- **解决** - 如果在调查风险检测之后在“标识保护”外部采取适当的补救措施，并且应该将风险检测视为已关闭，请将事件标记为“已解决”。 解决的事件会将风险检测的状态设置为“已关闭”，此风险检测不再算作用户风险。
- **标记为误报** - 在某些情况下，可以调查某个风险检测，查明该检测是否被错误地标记为有风险。 将风险检测标记为误报可以减少这种情况的发生次数。 这可以帮助机器学习算法将来改善类似事件的分类。 误报事件的状态为“已关闭”，不再算作用户风险。
- **忽略** - 如果尚未采取任何补救措施，但想要从活动列表中删除风险检测，可以将风险检测标记为“忽略”。在这种情况下，事件状态将变为“已关闭”。 忽略的事件不算作用户风险。 这种做法只能在非常情况下使用。
- **重新激活** - 可以（通过选择“解决”、“误报”或“忽略”）重新激活已手动关闭的风险检测，将事件状态重新设置为“活动”。 重新激活的风险检测包括在用户风险级别计算中。 无法重新激活通过补救措施（例如重置安全密码）关闭的风险检测。

## <a name="next-steps"></a>后续步骤

若要获取“Azure AD 标识保护”的概述，请参阅 [Azure AD 标识保护概述](overview.md)。

<!-- Update_Description: wording update -->