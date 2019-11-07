---
title: 在 Azure Active Directory B2C 中管理对资源和数据的威胁
description: 了解 Azure Active Directory B2C 中用于拒绝服务攻击和密码攻击的检测和缓解技术。
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
origin.date: 09/26/2019
ms.date: 10/24/2019
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: aadf173c187ec7a068b2f85d883eb30b870e5c5a
ms.sourcegitcommit: 817faf4e8d15ca212a2f802593d92c4952516ef4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72846975"
---
# <a name="manage-threats-to-resources-and-data-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中管理对资源和数据的威胁

Azure Active Directory B2C (Azure AD B2C) 具有内置功能，可以帮助你防御对资源和数据的威胁。 这些威胁包括拒绝服务攻击和密码攻击。 拒绝服务攻击可能会使目标用户无法使用资源。 密码攻击会导致未经授权的资源访问。

## <a name="denial-of-service-attacks"></a>拒绝服务攻击

Azure AD B2C 使用 SYN cookie 防御 SYN 洪流攻击。 Azure AD B2C 还使用速率和连接限制防止拒绝服务攻击。

## <a name="password-attacks"></a>密码攻击

要求用户所设密码的复杂性合理。 Azure AD B2C 针对密码攻击实施了缓解技术。 缓解措施包括检测暴力破解密码攻击和字典密码攻击。 Azure AD B2C 使用各种信号分析请求的完整性。 Azure AD B2C 旨在智能地将目标用户与黑客和僵尸网络区分开来。

Azure AD B2C 使用复杂策略来锁定帐户。 将根据请求的 IP 和输入的密码锁定帐户。 锁定的持续时间也会根据存在攻击的可能性而延长。 密码尝试 10 次失败后（默认尝试阈值），会进行一分钟锁定。 在帐户解锁后（即在锁定期限到期后由服务自动解锁帐户后）下一次登录失败时，将再次进行一分钟锁定，每次登录失败都将继续锁定。 重复输入相同的密码不会计为多次不成功登录。

前 10 个锁定期限的长度为一分钟。 接下来的 10 个锁定期限时间稍长，并且每 10 个锁定期限后都会增加持续时间。 当帐户未锁定时，锁定计数器在成功登录后重置为零。 锁定期限可以持续长达五个小时。

<!-- Update_Description: wording update -->