---
title: "在 Azure 多重身份验证中使用验证码登录到移动应用"
description: "本页介绍用户如何在 Azure MFA 中使用移动应用验证码登录。"
services: multi-factor-authentication
documentationCenter: 
authors: billmath
manager: stevenpo
editor: curtland
ms.service: multi-factor-authentication
ms.topic: article
origin.date: 08/04/2016
ms.date: 06/21/2017
ms.author: v-junlch
ms.openlocfilehash: 20f068b94741ff2d9b17953a97fdc36036197bde
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="signing-in-to-the-mobile-app-using-a-verification-code-with-azure-multi-factor-authentication"></a>在 Azure 多重身份验证中使用验证码登录到移动应用

以下信息介绍当你使用验证码执行身份验证时，在移动应用上使用多重身份验证的体验。

## <a name="to-sign-in-using-a-verification-code-with-your-mobile-app"></a>使用移动应用中的验证码登录

<ol>

<li>使用用户名和密码登录到 Office 365 等应用程序或服务。</li>
<li>Microsoft 将提示输入验证码。</li>

<center>![设置](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)</center>

<li>打开手机上的 Azure 验证器应用，然后在登录框中输入该代码。</li>

<center>![设置](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)</center>

<li>现在，应该已登录。</li>