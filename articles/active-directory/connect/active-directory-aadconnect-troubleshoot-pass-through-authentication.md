---
title: "Azure AD Connect：对直通身份验证进行故障排除 | Microsoft Docs"
description: "本文介绍如何对 Azure Active Directory (Azure AD) 直通身份验证进行故障排除。"
services: active-directory
keywords: "对 Azure AD Connect 直通身份验证进行故障排除, 安装 Active Directory, Azure AD 所需的组件, SSO, 单一登录"
documentationcenter: 
author: alexchen2016
manager: digimobile
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/15/2017
ms.date: 06/23/2017
ms.author: v-junlch
ms.openlocfilehash: 8882463ebf96017c6922e2983e4d410fd880b56f
ms.sourcegitcommit: a93ff901be297d731c91d77cd7d5c67da432f5d4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2017
---
# 对 Azure Active Directory 直通身份验证进行故障排除
<a id="troubleshoot-azure-active-directory-pass-through-authentication" class="xliff"></a>

本文有助于查找有关 Azure AD 直通身份验证相关的常见问题疑难解答信息。

>[!IMPORTANT]
>如果遇到直通身份验证方面的用户登录问题，在没有用于回退的仅限云的全局管理员帐户的情况下，请勿禁用该功能或卸载直通身份验证代理。 执行此步骤非常重要，可确保租户不会被锁定。

## 常规问题
<a id="general-issues" class="xliff"></a>

### 面向用户的登录错误消息
<a id="user-facing-sign-in-error-messages" class="xliff"></a>

如果用户无法使用直通身份验证登录，他们可能会在 Azure AD 登录屏幕上看到以下面向用户的错误之一： 

|错误|说明|解决方法
| --- | --- | ---
|AADSTS80001|无法连接到 Active Directory|确保代理服务器是需要验证其密码的用户所在的 AD 林的成员，并且能够连接到 Active Directory。  
|AADSTS8002|连接到 Active Directory 时超时|检查以确保 Active Directory 可用，并且可以响应代理的请求。
|AADSTS80004|传递到代理的用户名无效|确保用户尝试使用正确的用户名登录。
|AADSTS80005|验证遇到不可预知的 WebException|暂时性错误。 重试请求。 如果持续失败，请与 Microsoft 支持人员联系。
|AADSTS80007|与 Active Directory 通信时出错|检查代理日志以了解更多信息，并验证 Active Directory 是否按预期方式运行。


## 身份验证代理安装问题
<a id="authentication-agent-installation-issues" class="xliff"></a>

### 发生了意外的错误
<a id="an-unexpected-error-occurred" class="xliff"></a>

从服务器[收集代理日志](#collecting-pass-through-authentication-agent-logs)，然后联系 Microsoft 支持部门反映问题。

## 身份验证代理注册问题
<a id="authentication-agent-registration-issues" class="xliff"></a>

### 由于端口被阻止，身份验证代理注册失败
<a id="registration-of-the-authentication-agent-failed-due-to-blocked-ports" class="xliff"></a>

确保安装身份验证代理的服务器能够与我们的服务 URL 和[此处](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites)列出的端口通信。

### 由于令牌或帐户授权错误，身份验证代理注册失败
<a id="registration-of-the-authentication-agent-failed-due-to-token-or-account-authorization-errors" class="xliff"></a>

确保对所有 Azure AD Connect 或独立身份验证代理安装和注册操作使用仅限云的全局管理员帐户。 已启用 MFA 的全局管理员帐户存在一个已知问题；作为解决方法，请暂时关闭 MFA（只是为了完成操作）。

### 发生了意外的错误
<a id="an-unexpected-error-occurred" class="xliff"></a>

从服务器[收集代理日志](#collecting-pass-through-authentication-agent-logs)，然后联系 Microsoft 支持部门反映问题。

## 身份验证代理卸载问题
<a id="authentication-agent-uninstallation-issues" class="xliff"></a>

### 卸载 Azure AD Connect 时出现警告消息
<a id="warning-message-when-uninstalling-azure-ad-connect" class="xliff"></a>

如果在租户中启用了直通身份验证，则当尝试卸载 Azure AD Connect 时，会显示以下警告消息：“除非已在其他服务器上安装其他直通身份验证代理，否则用户无法登录到 Azure AD。”

在卸载 Azure AD Connect 之前，需确保设置为[高可用性](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability)，以免影响用户登录。

## 启用该功能方面的问题
<a id="issues-with-enabling-the-feature" class="xliff"></a>

### 由于没有可用的身份验证代理，启用该功能失败
<a id="enabling-the-feature-failed-because-there-were-no-authentication-agents-available" class="xliff"></a>

必须至少有一个活动的身份验证代理才能在租户中启用直通身份验证。 可以通过安装 Azure AD Connect 或独立身份验证代理来安装身份验证代理。

### 由于端口被阻止，启用该功能失败
<a id="enabling-the-feature-failed-due-to-blocked-ports" class="xliff"></a>

确保安装 Azure AD Connect 的服务器能够与我们的服务 URL 和[此处](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites)列出的端口通信。

### 由于令牌或帐户授权错误，启用该功能失败
<a id="enabling-the-feature-failed-due-to-token-or-account-authorization-errors" class="xliff"></a>

启用该功能时，确保使用仅限云的全局管理员帐户。 已启用多重身份验证 (MFA) 的全局管理员帐户存在一个已知问题；作为解决方法，请暂时关闭 MFA（只是为了完成操作）。

## 收集直通身份验证代理日志
<a id="collecting-pass-through-authentication-agent-logs" class="xliff"></a>

根据可能遇到的问题类型，需要在不同的位置查看直通身份验证代理日志。

### 身份验证代理事件日志
<a id="authentication-agent-event-logs" class="xliff"></a>

对于与身份验证代理相关的错误，请在服务器上打开事件查看器应用程序，然后检查 Application and Service Logs\Microsoft\AadApplicationProxy\Connector\Admin 中的日志。

若要获取详细的分析信息，请启用“会话”日志。 正常操作期间，请不要在启用此日志的情况下运行身份验证代理；只能将此日志用于故障排除。 日志内容只会在再次禁用日志后才可见。

### 详细跟踪日志
<a id="detailed-trace-logs" class="xliff"></a>

若要排查用户登录故障，请查看 \ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace 中的跟踪日志。 这些日志包含特定用户使用直通身份验证功能登录失败的原因。  下面是一个示例日志项目：

```
ApplicationProxyConnectorService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
    ThreadId=5
    DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

可以通过打开命令提示符并运行以下命令来获取错误（在上述示例中为“1328”）的描述性详细信息（请注意：将“1328”替换为日志中看到的实际错误编号）：

`Net helpmsg 1328`

![直通身份验证](./media/active-directory-aadconnect-pass-through-authentication/pta3.png)

### 域控制器日志
<a id="domain-controller-logs" class="xliff"></a>

如果已启用审核日志记录，可以在域控制器的安全日志中找到更多信息。 下面演示了一种查询直通身份验证代理发送的登录请求的简单方法：

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe')]]</Select>
    </Query>
    </QueryList>
```

