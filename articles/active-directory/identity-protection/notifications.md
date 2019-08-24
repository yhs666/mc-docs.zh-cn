---
title: “Azure Active Directory 标识保护”通知 | Microsoft Docs
description: 了解通知如何支持调查活动。
services: active-directory
keywords: Azure Active Directory 标识保护, Cloud App Discovery, 管理应用程序, 安全, 风险, 风险级别, 漏洞, 安全策略
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 12/07/2017
ms.date: 08/09/2019
ms.author: v-junlch
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9518a7f7b6e8225c21da405bc8e9b258196c736e
ms.sourcegitcommit: 44548f2ebec1246f6ac799f5b2640ad1b5d7c8a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2019
ms.locfileid: "68973330"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>“Azure Active Directory 标识保护”通知

Azure AD 标识保护会发送两种类型的自动生成的通知电子邮件，帮助你管理用户风险和风险事件：

- 检测到有风险的用户电子邮件
- 每周摘要电子邮件

本文概述了两种通知电子邮件。


## <a name="users-at-risk-detected-email"></a>检测到有风险的用户电子邮件

当“Azure AD 标识保护”检测到帐户受到威胁时，会生成“检测到有风险的用户”  的警报电子邮件。 建议立即调查有风险的用户。

此警报的配置允许你指定要生成警报的用户风险级别。 当用户的风险级别达到你指定的风险级别时，将生成电子邮件；但是，在用户达到此用户风险级别后，你将不会收到针对该用户的新的用户检测到风险电子邮件警报。 例如，如果你将策略设置为对中等用户风险发出警报，并且你的用户 John 达到中等风险，那么你将收到针对 John 的用户检测到风险电子邮件。 但是，如果 John 随后达到高风险或有其他风险事件，你将不会收到第二个用户检测到风险警报。

![检测到有风险的用户电子邮件](./media/notifications/01.png)


### <a name="configuration"></a>配置

管理员可以设置：

- **触发生成此电子邮件的用户风险级别** - 默认情况下，此风险级别设置为“高”风险。
- **此邮件的收件人** - 收件人默认包括所有全局管理员。 全局管理员还可将其他全局管理员、安全管理员、安全读取者添加为收件人。  


要打开相关对话框，请单击“标识保护”页中“设置”部分的“警报”    。

![检测到有风险的用户电子邮件](./media/notifications/05.png)


## <a name="weekly-digest-email"></a>每周摘要电子邮件

每周摘要电子邮件中包含新风险事件的摘要。  
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

