---
title: Azure AD 权利管理（预览版）中的请求过程和电子邮件通知 - Azure Active Directory
description: 了解访问包请求过程，以及 Azure Active Directory 权利管理（预览版）中何时发送电子邮件通知。
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
origin.date: 05/30/2019
ms.date: 08/09/2019
ms.author: v-junlch
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3268de9cd25e18792f64f60da8abfd1a2d6da204
ms.sourcegitcommit: 44548f2ebec1246f6ac799f5b2640ad1b5d7c8a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2019
ms.locfileid: "68972807"
---
# <a name="request-process-and-email-notifications-in-azure-ad-entitlement-management-preview"></a>Azure AD 权利管理（预览版）中的请求过程和电子邮件通知

> [!IMPORTANT]
> Azure Active Directory (Azure AD) 权利管理目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[适用于 Azure 预览版的补充使用条款](https://www.azure.cn/support/legal/)。

当用户将请求提交到访问包时，将启动一个传送该请求的过程。 如果在此过程中发生重要事件，Azure AD 权利管理还会向审批者和请求者发送电子邮件通知。

本文介绍请求过程以及发送的电子邮件通知。

## <a name="request-process"></a>请求过程

需要访问访问包的用户可以提交访问请求。 根据策略的配置，该请求可能需要审批。 批准请求后，将启动一个过程，以便为用户分配对访问包中每个资源的访问权限。 下图显示了该过程和不同状态的概览。

![审批过程示意图](./media/entitlement-management-process/request-process.png)

| 状态 | 说明 |
| --- | --- |
| 已提交 | 用户提交了请求。 |
| 待审批 | 如果访问包的策略需要审批，请求将转为“等待审批”状态。 |
| Expired | 如果在审批请求超时期限内没有任何审批者审批请求，该请求将会过期。 若要重试，用户必须重新提交请求。 |
| 已拒绝 | 审批者拒绝了请求。 |
| 已批准 | 审批者批准了请求。 |
| 传送 | **尚未**为用户分配对访问包中所有资源的访问权限。 如果这是一个外部用户，则表示用户尚未访问资源目录并接受权限提示。 |
| 已交货 | 已经为用户分配了对访问包中所有资源的访问权限。 |
| 访问权限已延期 | 如果策略中允许延期，则表示用户已延期分配。 |
| 访问权限已过期 | 用户对访问包的访问权限已过期。 若要重新获得访问权限，用户必须提交请求。 |

## <a name="email-notifications"></a>电子邮件通知

如果你是审批者，当你需要审批某个访问请求，以及某个访问请求已完成时，你将收到电子邮件通知。 如果你是请求者，你将会收到电子邮件通知，其中指出了请求的状态。 下图显示了何时发送这些电子邮件通知。

![权利管理电子邮件过程](./media/entitlement-management-process/email-notifications.png)

下表提供了有关这些电子邮件通知的更多详细信息。

| # | 电子邮件主题 | 发送时间 | 收件人 |
| --- | --- | --- | --- |
| 1 | 所需操作：评审 [请求者] 在 [日期] 对 [访问包] 发出的访问请求    | 当请求者提交访问包的请求时 | 所有审批者 |
| 2 | 所需操作：评审 [请求者] 在 [日期] 对 [访问包] 发出的访问请求    | 审批请求超时前的 X 天 | 所有审批者 |
| 3 | 状态通知: [请求者] 对 [访问包] 的访问请求已过期   | 当审批者在请求持续时间内未批准或拒绝访问请求时 | 请求者 |
| 4 | 状态通知: [请求者] 对 [访问包] 的访问请求已完成   | 当第一个审批者批准或拒绝访问请求时 | 所有审批者 |
| 5 | 已拒绝你访问 [访问包]  | 拒绝请求者访问访问包时 | 请求者 |
| 6 | 现在你可以访问 [访问包]   | 当授予请求者对访问包中每个资源的访问权限时 | 请求者 |
| 7 | 对 [访问包] 的访问权限将在 X 天后过期  | 请求者对访问包的访问权限过期之前的 X 天 | 请求者 |
| 8 | 对 [访问包] 的访问权限已过期  | 当请求者对访问包的访问权限过期时 | 请求者 |

### <a name="access-request-emails"></a>访问请求电子邮件

当请求者对配置为要求审批的访问包提交访问请求时，在策略中配置的所有审批者都会收到包含请求详细信息的电子邮件通知。 详细信息包括请求者的姓名、组织、访问开始和结束日期（如果已提供）、业务理由、提交请求时间以及请求过期时间。 审批者可以通过该电子邮件中的一个链接批准或拒绝访问请求。 下面是当请求者提交访问请求时发送给审批者的示例电子邮件通知。

![评审访问请求电子邮件](./media/entitlement-management-shared/email-approve-request.png)

### <a name="approved-or-denied-emails"></a>已批准或已拒绝电子邮件

当请求者的访问请求已获批准，允许其访问时，或者其访问请求遭到拒绝时，请求者将收到通知。 当审批者收到请求者提交的访问请求时，可以批准或拒绝该访问请求。 审批者需要添加其决策的业务理由。

批准访问请求时，权利管理将启动向请求者授予访问包中每个资源的访问权限的过程。 授予请求者对访问包中每个资源的访问权限后，将向请求者发送一封电子邮件通知，告知其访问请求已获批准，他们现在有权访问该访问包。 下面是在向请求者授予访问包的访问权限时，发送给请求者的示例电子邮件通知。

拒绝访问请求时，将向请求者发送电子邮件通知。 下面是拒绝请求者的访问请求时发送给请求者的示例电子邮件通知。

### <a name="expired-access-request-emails"></a>已过期访问请求电子邮件

当请求者的访问请求过期时，他们会收到通知。 当请求者提交访问请求时，该请求将附带一个持续时间，在此时间后即会过期。 如果没有任何审批者提交批准/拒绝决策，该请求将继续保持等待审批状态。 当请求达到配置的过期时间时，该请求将会过期，审批者不再可以批准或拒绝该请求。 在这种情况下，请求将进入过期状态。 不再可以批准或拒绝已过期的请求。 请求者会收到电子邮件通知，其中指出其访问请求已过期，需要重新提交。 下面是请求者的访问请求过期时发送给请求者的示例电子邮件通知。

![已过期访问请求电子邮件](./media/entitlement-management-process/email-expired-access-request.png)

## <a name="next-steps"></a>后续步骤

- [请求访问访问包](entitlement-management-request-access.md)
- [批准或拒绝访问请求](entitlement-management-request-approve.md)

