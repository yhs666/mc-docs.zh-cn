---
title: Azure Active Directory 标识保护风险检测参考 | Microsoft Docs
description: Azure Active Directory 标识保护风险检测参考。
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: reference
origin.date: 01/25/2018
ms.date: 10/10/2019
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: bbd2a9e458d4ae6951a6f5c62c76e8a60a4eadd6
ms.sourcegitcommit: 74f50c9678e190e2dbb857be530175f25da8905e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2019
ms.locfileid: "72292076"
---
# <a name="azure-active-directory-identity-protection-risk-detections-reference"></a>Azure Active Directory 标识保护风险检测参考

大多数安全违规出现在当攻击者通过窃取用户的标识来获取环境的访问权限时。 发现标识是否遭到入侵并不容易。 Azure Active Directory 使用自适应机器学习算法和试探法来检测与用户帐户相关的可疑操作。 每个检测到的可疑操作都存储在称为“风险检测”的记录中。

## <a name="anonymous-ip-address"></a>匿名 IP 地址

**检测类型：** 实时  
**旧名称：** 从匿名 IP 地址登录

此风险检测类型指示从匿名 IP 地址登录（例如，Tor 浏览器，匿名程序 VPN）。
这些 IP 地址通常由希望隐藏其登录遥测数据（IP 地址、位置、设备等）的参与者使用以实现潜在恶意目的。

## <a name="atypical-travel"></a>异常位置登录

**检测类型：** 脱机  
**旧名称：** 不可能前往异常位置

此风险检测类型可标识从相距遥远的地理位置进行的两次登录，根据用户以往的行为，其中至少有一个位置属于异常。 除了若干其他因素外，此机器学习算法还考虑两次登录之间相隔的时间以及用户从第一个位置前往第二个位置所需的时间，因为这指示有不同的用户在使用相同的凭据。

此算法会忽略明显的“误报”，从而改善不可能前往条件，例如组织中其他用户定期使用的 VPN 和位置。 系统最早有一个 14 天或 10 次登录的初始学习期限，在此期间它将学习新用户的登录行为。

## <a name="leaked-credentials"></a>凭据泄漏

**检测类型：** 脱机  
**旧名称：** 具有已泄漏凭据的用户

此风险检测类型指示用户的有效凭据已泄漏。
当网络犯罪分子泄露合法用户的有效密码时，他们通常会共享这些凭据。 共享方式通常为将凭据公开发布在暗网或粘贴网站上，或者在黑市上交易或出售凭据。 Microsoft 泄露凭据服务通过监控公网和暗网并与以下机构合作获取用户名/密码对：

- 研究人员
- 执法机构
- Microsoft 安全团队
- 其他受信任的来源

当服务从暗网、粘贴网站或上述来源获取用户凭据时，会根据 Azure AD 用户当前的有效凭据对其进行检查，以找到有效匹配项。

## <a name="malware-linked-ip-address"></a>受恶意软件感染的 IP 地址

**检测类型：** 脱机  
**旧名称：** 从受感染的设备登录

此风险检测类型指示从感染了恶意软件的 IP 地址（即主动与僵尸服务器通信）登录。 这通过分析用户设备的 IP 地址与僵尸服务器活动时连接过僵尸服务器的 IP 地址之间的相互关系可得以确定。

## <a name="unfamiliar-sign-in-properties"></a>不熟悉的登录属性

**检测类型：** 实时  
**旧名称：** 从不熟悉的位置登录

此风险检测类型会根据过去的登录历史记录（IP、纬度/经度和 ASN）来查找异常登录。系统会存储用户以前的登录位置信息，并将其视为“熟悉”位置。 当从尚未在熟悉位置列表中列出的位置登录时，将触发此风险检测。 新创建的用户将在一段时间内处于“学习模式”，在此期间，当我们的算法学习用户的行为时，将关闭不熟悉的登录属性风险检测。 学习模式持续时间是动态的，取决于算法收集足够的用户登录模式信息所需的时间。 最短持续时间为五天。 用户可以在长时间处于非活动状态后返回到学习模式。 系统还会忽略从常用设备和接近熟悉位置的地理位置进行登录。 

我们还对基本身份验证（或旧版协议）运行此检测。 由于这些协议没有新型属性（如客户端 ID），因此用来减少误报的遥测数据较为有限。 建议客户采用新式身份验证。

## <a name="azure-ad-threat-intelligence"></a>Azure AD 威胁智能

**检测类型：** 脱机 <br>
**旧名称：** 此检测将在旧版 Azure AD 标识保护报告（标记为有风险、有风险检测的用户）中显示为“凭据泄露的用户”

此风险检测类型指示用户活动对于给定用户来说是不寻常的，或者根据 Microsoft 内部和外部威胁智能源与已知攻击模式相一致。

## <a name="admin-confirmed-user-compromised"></a>管理员确认用户遭入侵

**检测类型：** 脱机 <br>
此检测表明管理员已通过风险用户 UI 或 riskyUsers API 选择了“确认用户遭入侵”。 若要查看哪位管理员确认了此用户遭入侵，请（通过 UI 或 API）检查用户的风险历史记录。

## <a name="malicious-ip-address"></a>恶意 IP 地址

**检测类型：** 脱机 <br>
此检测表明登录是通过恶意 IP 地址进行的。 根据以下标准确定 IP 地址是否是恶意的：
-   故障率高（原因是从 IP 地址收到的凭据无效）
-   其他 IP 信誉源

## <a name="additional-risk-detected"></a>检测到其他风险

**检测类型：** 实时或脱机 <br>
此检测表明已检测到上述某个高级检测。 由于高级检测仅显示给 Azure AD 高级版 P2 客户，因此其标题为“检测到其他风险”（针对非 P2 客户）。

<!-- Update_Description: wording update -->