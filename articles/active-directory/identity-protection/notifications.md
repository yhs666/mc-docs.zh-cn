---
title: “Azure Active Directory 标识保护”通知 | Microsoft Docs
description: 了解通知如何支持调查活动。
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
origin.date: 12/07/2017
ms.date: 10/10/2019
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7dde9b193b7f59974f69b09b1ae67390dbf77d78
ms.sourcegitcommit: 74f50c9678e190e2dbb857be530175f25da8905e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2019
ms.locfileid: "72292081"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>“Azure Active Directory 标识保护”通知

Azure AD 标识保护会发送两种类型的自动生成的通知电子邮件，帮助你管理用户风险和风险检测：

- 检测到有风险的用户电子邮件
- 每周摘要电子邮件

本文概述了两种通知电子邮件。

## <a name="users-at-risk-detected-email"></a>检测到有风险的用户电子邮件

当“Azure AD 标识保护”检测到帐户受到威胁时，会生成“检测到有风险的用户”  的警报电子邮件。 建议立即调查有风险的用户。

此警报的配置允许你指定要生成警报的用户风险级别。 当用户的风险级别达到你指定的风险级别时，将生成电子邮件；但是，在用户达到此用户风险级别后，你将不会收到针对该用户的新的用户检测到风险电子邮件警报。 例如，如果你将策略设置为对中等用户风险发出警报，并且你的用户 John 达到中等风险，那么你将收到针对 John 的用户检测到风险电子邮件。 但是，如果 John 随后达到高风险或进行了其他风险检测，你将不会收到第二个“检测到存在风险的用户”警报。

![检测到有风险的用户电子邮件](./media/notifications/01.png)

### <a name="configuration"></a>配置

管理员可以设置：

- **触发生成此电子邮件的用户风险级别** - 默认情况下，此风险级别设置为“高”风险。
- **此邮件的收件人** - 收件人默认包括所有全局管理员。 全局管理员还可将其他全局管理员、安全管理员、安全读取者添加为收件人。  

要打开相关对话框，请单击“标识保护”页中“设置”部分的“警报”    。

![检测到有风险的用户电子邮件](./media/notifications/05.png)

## <a name="weekly-digest-email"></a>每周摘要电子邮件

每周摘要电子邮件中包含新风险检测的摘要。  
其中包括：

- 有风险的用户
- 可疑活动
- 检测到的漏洞
- 指向“标识保护”中相关报告的链接

    ![补救](./media/notifications/400.png "补救")

### <a name="configuration"></a>配置

管理员可以关闭发送每周摘要电子邮件。

![用户风险](./media/notifications/62.png "用户风险")

要打开相关对话框，请单击“标识保护”页中“设置”部分的“每周摘要”    。

![检测到有风险的用户电子邮件](./media/notifications/04.png)

## <a name="see-also"></a>另请参阅

- [Azure Active Directory 标识保护](/active-directory/identity-protection/overview)

<!-- Update_Description: wording update -->