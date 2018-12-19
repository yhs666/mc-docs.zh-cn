---
title: Azure Active Directory 概念证明操作手册：构建基块 | Microsoft Docs
description: 研究并快速实现标识和访问管理方案
services: active-directory
keywords: azure active directory 操作手册, 概念证明, PoC
documentationcenter: ''
author: dstefanMSFT
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/04/2017
ms.date: 10/12/2018
ms.author: v-junlch
ms.openlocfilehash: 6f516f82fa268f2ac3391888eb00923f88594c06
ms.sourcegitcommit: 6cd0a8d22061aba7390579a80e19cb9d2f7faf12
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "53233780"
---
# <a name="azure-active-directory-proof-of-concept-playbook-building-blocks"></a>Azure Active Directory 概念证明操作手册：构建基块

## <a name="catalog-of-roles"></a>角色的目录

| 角色 | 说明 | 概念证明 (PoC) 责任 |
| --- | --- | --- |
| 标识体系结构/开发团队 | 此团队通常负责设计解决方案、实现原型、推动审批但最终不涉及运营 | 他们提供环境，并从可管理性角度评估不同的方案 |
| **本地标识运营团队** | 管理不同的本地标识源：Active Directory 林、LDAP 目录、HR 系统和联合身份验证标识提供程序。 | 提供 PoC 方案所需的对本地资源的访问权限。<br/>他们应尽量少涉及|
| **应用程序技术所有者** | 要与 Azure AD 集成的不同云应用和服务的技术所有者 | 提供有关 SaaS 应用程序（可能是用于测试的实例）的详细信息 |
| **Azure AD 全局管理员** | 管理 Azure AD 配置 | 提供用于配置同步服务的凭据。 在 PoC 过程中通常与标识体系结构是同一团队，但是在运营阶段通常是独立团队|
| **数据库团队** | 数据库基础结构的所有者 | 提供对 SQL 环境（ADFS 或 Azure AD Connect）的访问权限，为特定方案做准备工作。<br/>应该尽量减少与此团队的交互 |
| **网络团队** | 网络基础结构的所有者 | 提供同步服务器在网络级别的所需访问权限，以便正常访问数据源和云服务（防火墙规则、打开的端口、IPSec 规则等） |
| 安全团队 | 定义安全策略、分析来自各种源的安全报告并遵循分析结果。 | 提供目标安全评估方案 |

## <a name="common-prerequisites-for-all-building-blocks"></a>所有构建基块的常见先决条件

| 先决条件 | 资源 |
| --- | --- |
| 使用有效的 Azure 订阅定义的 Azure AD 租户 | [如何获取 Azure Active Directory 租户](develop/quickstart-create-new-tenant.md)|
| Azure AD 全局管理员凭据 | [在 Azure Active Directory 中分配管理员角色](active-directory-assign-admin-roles-azure-portal.md) |
| 可选但强烈建议使用：作为回退措施的并行实验室环境 | [Azure AD Connect 的先决条件](./connect/active-directory-aadconnect-prerequisites.md) |

<a name="directory-synchronization---password-hash-sync-phs---new-installation"></a>
## <a name="directory-synchronization--password-hash-sync-phs--new-installation"></a>目录同步 - 密码哈希同步 (PHS) - 新安装
估计完成时间：低于 1,000 位 PoC 用户的情况下大约一小时

### <a name="pre-requisites"></a>先决条件

| 先决条件 | 资源 |
| --- | --- |
| 运行 Azure AD Connect 的服务器 | [Azure AD Connect 的先决条件](./connect/active-directory-aadconnect-prerequisites.md) |
| 目标 POC 用户，在同一域中，并且属于安全组和 OU | [Azure AD Connect 的自定义安装](./hybrid/how-to-connect-install-custom.md#domain-and-ou-filtering) |
| 已具有本地和云环境所需的凭据  | [Azure AD Connect：帐户和权限](./connect/active-directory-aadconnect-accounts-permissions.md) |

### <a name="steps"></a>步骤

| 步骤 | 资源 |
| --- | --- |
| 下载最新版本的 Azure AD Connect | [下载 Azure Active Directory Connect](https://www.microsoft.com/download/details.aspx?id=47594) |
| 使用最简单的路径安装 Azure AD Connect：Express <br/>1.筛选出目标 OU，以最大限度地缩短同步周期时间<br/>2.在本地组中选择目标用户群。<br/>3.部署其他 POC 主题需要的功能 | [Azure AD Connect：自定义安装：域和 OU 筛选](./hybrid/how-to-connect-install-custom.md#domain-and-ou-filtering) <br/>[Azure AD Connect：自定义安装：基于组的筛选](./hybrid/how-to-connect-install-custom.md#sync-filtering-based-on-groups)<br/>
| 打开 Azure AD Connect UI，并查看已完成的运行配置文件（导入、同步和导出） | [Azure AD Connect 同步：计划程序](./connect/active-directory-aadconnectsync-feature-scheduler.md) |

### <a name="considerations"></a>注意事项

1. 请在[此处](./connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md)查看密码哈希同步的安全注意事项。  如果确实不能选择试点生产用户的密码哈希同步，请考虑使用以下替代方法：
   - 在生产域中创建测试用户。 确保不会同步任何其他帐户
   - 转到 UAT 环境
2.  如果想要追求联合，有必要了解使用本地标识提供者关联联合解决方案以及 POC 的成本，并根据想要获得的优势权衡这种做法：
    - 它是关键路径，因此应该设计为高可用性
    - 它是容量计划所需的本地服务
    - 它是需要进行监视/维护/修补的本地服务

了解更多：[了解 Office 365 标识和 Azure Active Directory - 联合身份](https://support.office.com/article/Understanding-Office-365-identity-and-Azure-Active-Directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9#bk_federated)

## <a name="generic-ldap-connector-configuration"></a>泛型 LDAP 连接器配置

估计完成时间：60 分钟

> [!IMPORTANT]
> 这是一项高级配置，需要对 FIM/MIM 有一定的了解。 如果在生产环境中使用，我们建议浏览[顶级支持](https://support.microsoft.com/premier)了解有关此配置的问题。

### <a name="pre-requisites"></a>先决条件

| 先决条件 | 资源 |
| --- | --- |
| 已安装并配置 Azure AD Connect | 构建基块：[目录同步 - 密码哈希同步](#directory-synchronization--password-hash-sync-phs--new-installation) |
| 满足要求的 ADLDS 实例 | 泛型 LDAP 连接器技术参考：泛型 LDAP 连接器概述 |
| 用户使用的工作负荷列表，以及与这些工作负荷关联的属性列表 | [Azure AD Connect 同步：与 Azure Active Directory 同步的属性](./connect/active-directory-aadconnectsync-attributes-synchronized.md) |


### <a name="steps"></a>步骤

| 步骤 | 资源 |
| --- | --- |
| 添加泛型 LDAP 连接器 | 泛型 LDAP 连接器技术参考：创建新连接器 |
| 为已创建的连接器创建运行配置文件（完全导入、增量导入、完全同步、增量同步、导出） | [创建管理代理运行配置文件](https://technet.microsoft.com/library/jj590219(v=ws.10).aspx)<br/> [将连接器与 Azure AD Connect Sync Service Manager 配合使用](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md)|
| 运行完全导入配置文件，并验证在连接器空间中是否存在对象 | [搜索连接器空间对象](https://technet.microsoft.com/library/jj590287(v=ws.10).aspx)<br/>[将连接器与 Azure AD Connect Sync Service Manager 配合使用：搜索连接器空间](./hybrid/how-to-connect-sync-service-manager-ui-connectors.md#search-connector-space) |
| 创建同步规则，使 Metaverse 中的对象具有工作负荷的所需属性 | [Azure AD Connect 同步：更改默认配置的最佳做法：同步规则的更改](./hybrid/how-to-connect-sync-best-practices-changing-default-configuration.md#changes-to-synchronization-rules)<br/>[Azure AD Connect 同步：了解声明性预配](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning.md)<br/>[Azure AD Connect 同步：了解声明性设置表达式](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| 启动完全同步周期 | [Azure AD Connect 同步：计划程序：启动计划程序](./hybrid/how-to-connect-sync-feature-scheduler.md#start-the-scheduler) |
| 发生问题时执行故障排除 | [排查对象无法同步到 Azure AD 的问题](./connect/active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) |
| 确认 LDAP 用户可以登录并访问应用程序 | https://login.partner.microsoftonline.cn |

### <a name="considerations"></a>注意事项

> [!IMPORTANT]
> 这是一项高级配置，需要对 FIM/MIM 有一定的了解。 如果在生产环境中使用，我们建议通过[顶级支持](https://support.microsoft.com/premier)解答有关此配置的问题。

## <a name="azure-multi-factor-authentication-with-phone-calls"></a>通过电话呼叫进行 Azure 多重身份验证

估计完成时间：10 分钟

### <a name="pre-requisites"></a>先决条件

| 先决条件 | 资源 |
| --- | --- |
| 确定要使用 MFA 的 POC 用户  |  |
| 用于接收 MFA 质询的信号良好的电话  | [什么是 Azure 多重身份验证？](authentication/multi-factor-authentication.md) |

### <a name="steps"></a>Steps

| 步骤 | 资源 |
| --- | --- |
| 在 Azure AD 门户中导航到“用户和组”边栏选项卡 | [Azure AD 门户：用户和组](https://portal.azure.cn/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Overview/menuId/) |
| 选择“所有用户”边栏选项卡 |  |
| 在顶栏中选择“多重身份验证”按钮 | Azure MFA 门户的的直接 URL： https://aka.ms/mfaportal |
| 在“用户”设置中选择 PoC 用户，并为其启用 MFA | [Azure 多重身份验证中的用户状态](authentication/howto-mfa-userstates.md) |
| 以 PoC 用户身份登录，并完成验证过程  |  |

### <a name="considerations"></a>注意事项

1. 此构建基块中的 PoC 步骤为用户的所有登录名明确设置了 MFA。  
2. 为方便起见，本构建基块中的 PoC 步骤明确使用电话呼叫作为 MFA 方法。 从 POC 转换为生产时，建议使用 [Microsoft Authenticator](../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md) 等应用程序作为第二个考虑的因素（如果需要）。
了解更多：[DRAFT NIST Special Publication 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html)（DRAFT NIST 特别发布 800-63B）

## <a name="configuring-certificate-based-authentication"></a>配置基于证书的身份验证

估计完成时间：20 分钟

### <a name="pre-requisites"></a>先决条件

| 先决条件 | 资源 |
| --- | --- |
| 已从企业 PKI 预配了用户证书的设备（Windows、iOS 或 Android） | [部署用户证书](https://msdn.microsoft.com/library/cc770857.aspx) |
| 与 ADFS 联合的 Azure AD 域 | [Azure AD Connect 和联合身份验证](./connect/active-directory-aadconnectfed-whatis.md)<br/>[Active Directory 证书服务概述](https://technet.microsoft.com/library/hh831740.aspx)|
| iOS 设备需要安装 Microsoft Authenticator 应用 | [Microsoft Authenticator 应用入门](user-help/microsoft-authenticator-app-how-to.md) |

### <a name="steps"></a>Steps

| 步骤 | 资源 |
| --- | --- |
| 在 ADFS 上启用“证书身份验证” | [配置身份验证策略：在 Windows Server 2012 R2 中全局配置主身份验证](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-authentication-policies#to-configure-primary-authentication-globally-in-windows-server-2012-r2) |
| 可选：在适用于 Exchange Active Sync 客户端的 Azure AD 中启用证书身份验证 | [Azure Active Directory 中基于证书的身份验证入门](./authentication/active-directory-certificate-based-authentication-get-started.md) |
| 导航到“访问面板”并使用“用户证书”进行身份验证 | https://login.partner.microsoftonline.cn |

### <a name="considerations"></a>注意事项

若要详细了解此部署的注意事项，请访问：[ADFS：通过 Azure AD 和 Office 365 进行证书身份验证](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)



> [!NOTE]
> 应保证拥有用户证书。 可以通过管理设备或使用 PIN（在智能卡的情况下）。



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]

<!-- Update_Description: link update -->