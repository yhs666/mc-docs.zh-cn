---
title: "Microsoft Authenticator 应用的帮助和支持 | Microsoft Docs"
description: "提供与 Microsoft Authenticator 应用和 Azure Multi-Factor Authentication 相关的常见问题与解答列表。"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: pblachar, librown
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: a114d832e9c5320e9a109c9020fcaa2f2fdd43a9
ms.openlocfilehash: d9279ce9c0688c09d643f011fd7bdecb6559214f
ms.lasthandoff: 04/14/2017


---
# <a name="microsoft-authenticator-app-faq"></a>Microsoft 验证器应用常见问题

本文解答我们收到的有关 Microsoft 验证器应用的常见问题。 如果你的问题在此处没有解答，请访问 [Microsoft 验证器应用论坛](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp)。 我们还提供了另一个有关应用上一个特定功能的常见问题解答，即[使用手机登录常见问题解答](microsoft-authenticator-app-phone-signin-faq.md)。

Microsoft Authenticator 应用替代了 Azure Authenticator 应用，建议使用 Azure Multi-Factor Authentication 时使用该应用。 Microsoft Authenticator 应用可用于 [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)、[Android](http://go.microsoft.com/fwlink/?Linkid=825072) 和 [IOS](http://go.microsoft.com/fwlink/?Linkid=825073)。

## <a name="frequently-asked-questions"></a>常见问题

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>此应用中的代码有哪些用途？ 为什么数字会保持倒计数？

打开 Microsoft 验证器应用时，将看到已添加的帐户以及按每个已添加的帐户列出的六位或八位数字。 你可能会看到倒计时的三十秒计时器。

登录到帐户时，将使用这些代码。 输入用户名和密码后，可能需要输入验证码。 打开 Microsoft 验证器应用，并复制当前显示的代码。 在登录页中输入该代码，以完成登录。

代码每隔 30 秒更改一次的原因是，使你永远不会使用同一代码两次。 它不像你应记住的密码。 其思想是，只有有权访问你的手机的人员知道你的验证码。

这些代码不需要 Internet 或数据，因此你不必担心使用电话服务进行登录，也不必担心应用将用完你的流量套餐。 关闭该应用后，该应用不会在后台继续运行，因此不会耗尽你的电池。 可以关闭该应用，并在下次登录之前忽略它。  

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>我已使用 Microsoft Authenticator 应用程序生成验证代码。 如何切换到一键式推送通知？
通过推送通知批准登录仅适用于个人 Microsoft 帐户或工作和学校 Microsoft 帐户，而不适用于 Google 或 Facebook 等第三方帐户。 如果用户拥有工作或学校 Microsoft 帐户，用户所在组织可选择禁用此选项。

如果使用 Microsoft 帐户作为个人帐户，并且想要切换到推送通知，需要再次添加帐户。 向您的帐户重新注册该设备，并设置推送通知。  

如果对工作或学校帐户使用 Microsoft Authenticator，那么组织可决定是否允许使用一键式通知。

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>一键式推送通知是否适用于非 Microsoft 帐户？
不适用，推送通知只适用于 Microsoft 帐户和 Azure Active Directory 帐户。 如果您的工作单位或学校使用的是 Azure AD 帐户，则可能会禁用此功能。  

### <a name="i-restored-my-device-from-a-backup-and-my-account-codes-are-missing-or-not-working-what-happened"></a>从备份还原设备后，帐户代码丢失或失效。 发生了什么情况？
出于安全考虑，我们不会从应用备份还原帐户。  还原应用后，请删除并再次添加帐户。

### <a name="i-got-a-new-device-how-do-i-remove-the-microsoft-authenticator-app-from-my-old-device-and-move-to-the-new-one"></a>我有了一台新设备。 如何从旧设备删除 Microsoft Authenticator 应用并将其迁移到新设备？
将 Microsoft Authenticator 应用添加到新设备不会从任何其他设备将其自动删除。 若要管理为帐户配置的设备，请访问用于管理双重验证的网站，选择删除旧应用。

对于个人 Microsoft 帐户，此网站即 [帐户安全](https://account.microsoft.com/security) 页面。 对于工作或学校 Microsoft 帐户，此网站可能是 [MyApps](https://login.partner.microsoftonline.cn) 或组织已设置的自定义门户。

### <a name="how-do-i-remove-an-account-from-the-app"></a>如何从应用删除帐户？
- iOS：从主屏幕中，在帐户磁贴上向左轻扫。 选择“删除”。
- Windows Phone：在主屏幕中，依次选择菜单按钮、“**编辑帐户**”。 点击帐户名称旁边的 **X**。
- Android：在主屏幕中，依次选择菜单按钮、“**编辑帐户**”。 点击帐户名称旁边的 **X**。

如果拥有已注册到组织的设备，可能需要完成一个额外步骤才能删除帐户。 在这些设备上，以设备管理员自动注册了 Microsoft Authenticator 应用。 如果想要彻底卸载应用，需受先在应用设置中取消注册应用。

### <a name="why-does-the-app-request-so-many-permissions"></a>应用为什么要请求如此多的权限？
下面是所需的权限以及如何在应用中使用这些权限的完整列表。 会出现哪些特定的权限将取决于所用的手机类型。

- **相机**：用户添加工作、学校或非 Microsoft 帐户时，应用使用相机来扫描 QR 码。
- **联系人和手机**：用户使用个人 Microsoft 帐户登录时，应用将尝试通过查找手机上使用的现有帐户来简化过程。
- **SMS**：用户首次使用个人 Microsoft 帐户登录时，应用必须确保手机号码与已记录的号码相匹配。 我们将向用于下载应用的手机发送短信。 该短信包含 6-8 位的验证码。 应用不会要求用户查找并在应用中输入此验证码，而是在短信中自动查找它。
- **在其他应用上方显示**：收到通知以验证身份时，在任何可能正在运行的其他应用上显示该通知。
- **从 Internet 接收数据**：需要此权限才能发送通知。
- **阻止手机进入睡眠状态**：如果用户将设备注册到组织，则组织可以更改其手机上的这一策略。
- **控制振动**：用户可以选择收到验证身份的通知时是否振动。
- **使用指纹硬件**：某些工作和学校帐户在验证身份时需要其他 PIN。 为简化此过程，我们允许使用指纹而不输入 PIN。
- **查看网络连接**：应用需要网络/Internet 连接才能添加 Microsoft 帐户。
- **读取存储的内容**：仅当通过应用设置报告技术问题时，才会使用此权限。 将从存储中收集某些信息来诊断问题。
- **完全网络访问**：此权限为发送验证身份的通知所需。
- **在启动时运行**：如果用户重启手机，此权限可确保继续接收验证身份的通知。

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>为何 Microsoft 验证器应用允许在不解锁设备的情况下批准请求？

这是设计使然。 双重验证要求提供两项证明：你知道的事和你拥有的物品。 你知道的事是密码。 你拥有的物品是你的手机（已在 Microsoft 验证器应用中经过设置，并已注册为 MFA 证明）。因此，拥有手机和批准请求符合第二个身份验证因素的条件。 

## <a name="next-steps"></a>后续步骤

### <a name="contact-us"></a>联系我们
如果在此处找不到问题的答案，请告诉我们。 转到 [Microsoft 验证器应用论坛](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) 发布问题并从社区获取帮助或者在此页面上留言，我们会尽快解答相关问题。


### <a name="related-topics"></a>相关主题
- [双重验证](https://support.microsoft.com/zh-cn/help/12408/microsoft-account-about-two-step-verification) 
- 为工作或学校帐户[进行设置双重验证时遇到问题](multi-factor-authentication-end-user-troubleshoot.md)？
- [使用 Microsoft Authenticator 通过手机登录](microsoft-authenticator-app-phone-signin-faq.md)


