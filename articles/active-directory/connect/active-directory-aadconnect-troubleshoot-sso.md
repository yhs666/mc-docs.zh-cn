---
title: "Azure AD Connect：无缝单一登录故障排除 | Microsoft Docs"
description: "本主题介绍了如何排除 Azure Active Directory 无缝单一登录（Azure AD 无缝 SSO）故障。"
services: active-directory
keywords: "什么是 Azure AD Connect, 安装 Active Directory, Azure AD 所需的组件, SSO, 单一登录"
documentationcenter: 
author: alexchen2016
manager: digimobile
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/12/2017
ms.date: 07/31/2017
ms.author: v-junlch
ms.openlocfilehash: 4025266e07ffb47f15291c599a2f1932a00224d5
ms.sourcegitcommit: cd0f14ddb0bf91c312d5ced9f38217cfaf0667f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>排除 Azure Active Directory 无缝单一登录故障

本文有助于查找有关 Azure AD 无缝单一登录相关的常见问题疑难解答信息。

## <a name="known-issues"></a>已知问题

- 如果要同步 30 个或更多 AD 林，则不能使用 Azure AD Connect 启用无缝 SSO。 作为一种解决方法，可以在租户中[手动启用](#manual-reset-of-azure-ad-seamless-sso)该功能。
- 将 Azure AD 服务 URL（https://autologon.microsoftazuread-sso.com、https://aadg.chinacloudapi.cn.nsatc.net）添加到“受信任的站点”区域，而不是阻止用户登录的“本地 Intranet”区域。
- 无缝 SSO 无法在 Firefox 的隐私浏览模式下正常工作。

## <a name="troubleshooting-checklist"></a>故障排除清单

使用以下清单排查无缝 SSO 问题：

- 检查 Azure AD Connect 中是否已启用无缝 SSO 功能。 如果无法启用该功能（例如，由于端口被阻止），请确保事先满足所有[先决条件](active-directory-aadconnect-sso-quick-start.md#step-1-check-prerequisites)。
- 检查这两个 Azure AD URL（https://autologon.microsoftazuread-sso.com 和 https://aadg.chinacloudapi.cn.nsatc.net）是否都是用户 Intranet 区域设置的一部分。
- 确保企业设备已加入 AD 域。
- 确保用户已使用 AD 域帐户登录设备。
- 确保用户的帐户来自已设置了无缝 SSO 的 AD 林。
- 确保已在企业网络中连接该设备。
- 确保设备的时间与 Active Directory 同步，并且各域控制器的时间偏差不超过 5 分钟。
- 使用命令提示符中的 klist 命令列出设备中现有的 Kerberos 票证。 检查是否存在为 `AZUREADSSOACCT` 计算机帐户颁发的票证。 用户的 Kerberos 票证通常有效期为 12 小时。 Active Directory 中可能有不同的设置。
- 使用 klist purge 命令清除设备中现有的 Kerberos 票证，然后重试。
- 若要确定是否存在与 JavaScript 相关的问题，请查看浏览器的控制台日志（在“开发人员工具”下）。
- 查看[域控制器日志](#domain-controller-logs)。

### <a name="domain-controller-logs"></a>域控制器日志

如果域控制器上已成功启用审核，则每当用户使用无缝 SSO 登录时，将在事件日志中记录一个安全条目。 可以使用以下查询查找这些安全事件（查找与计算机帐户 AzureADSSOAcc$ 相关联的事件 4769）：

```
<QueryList>
    <Query Id="0" Path="Security">
        <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
    </Query>
</QueryList>
```

## <a name="manual-reset-of-azure-ad-seamless-sso"></a>手动重置 Azure AD 无缝 SSO

如果故障排除不起作用，请使用以下步骤在租户中手动重置该功能：

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>步骤 1：导入无缝 SSO PowerShell 模块

1. 首先，下载并安装 [Microsoft Online Services 登录助手](http://go.microsoft.com/fwlink/?LinkID=286152)。
2. 然后下载并安装[用于 Windows PowerShell 的 64 位 Azure Active Directory 模块](http://go.microsoft.com/fwlink/p/?linkid=236297)。
3. 导航到 `%programfiles%\Azure Active Directory Connect` 文件夹。
4. 使用以下命令导入无缝 SSO PowerShell 模块：`Import-Module .\AzureADSSO.psd1`。

### <a name="step-2-get-the-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>步骤 2：获取已在其中启用了无缝 SSO 的 AD 林列表

1. 在 PowerShell 中，调用 `New-AzureADSSOAuthenticationContext`。 出现提示时，输入 Azure AD 租户管理员凭据。
2. 调用 `Get-AzureADSSOStatus`。 此命令提供已在其中启用了此功能的 AD 林列表（请查看“域”列表）。

### <a name="step-3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>步骤 3：为已设置无缝 SSO 的每个 AD 林禁用该功能

1. 调用 `$creds = Get-Credential`。 出现提示时，输入目标 AD 林的域管理员凭据。
2. 调用 `Disable-AzureADSSOForest -OnPremCredentials $creds`。 此命令会从此特定 AD 林的本地域控制器中删除 `AZUREADSSOACCT` 计算机帐户。
3. 针对已设置此功能的每个 AD 林重复上述步骤。

### <a name="step-4-enable-seamless-sso-for-each-ad-forest"></a>步骤 4：为每个 AD 林启用无缝 SSO

1. 调用 `Enable-AzureADSSOForest`。 出现提示时，输入目标 AD 林的域管理员凭据。
2. 针对想要设置此功能的每个 AD 林重复上述步骤。

### <a name="step-5-enable-the-feature-on-your-tenant"></a>步骤 5。 在租户上启用该功能

调用 `Enable-AzureADSSO` 并在 `Enable: ` 提示符处键入“true”，在租户中启用此功能。

<!-- Update_Description: update meta properties -->