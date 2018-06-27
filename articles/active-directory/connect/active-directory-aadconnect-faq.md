---
title: Azure Active Directory Connect：常见问题解答 - | Microsoft Docs
description: 此页包含有关 Azure AD Connect 的常见问题。
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/05/2018
ms.date: 06/22/2018
ms.component: hybrid
ms.author: v-junlch
ms.openlocfilehash: 4207d4f5c28d1c4533cfba17c80ec56527b6b437
ms.sourcegitcommit: d744d18624d2188adbbf983e1c1ac1110d53275c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314291"
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>有关 Azure Active Directory Connect 的常见问题

## <a name="general-installation"></a>常规安装
**问：如果 Azure AD 全局管理员已启用 2FA，安装是否能够正常进行？**  
2016 年 2 月版本开始支持此方案。

**问：Azure AD Connect 是否提供无人值守安装方法？**  
仅支持使用安装向导来安装 Azure AD Connect。 不支持无人值守和静默安装。

**问：我有一个林，但无法连接到其中的某个域。如何安装 Azure AD Connect？**  
2016 年 2 月版本开始支持此方案。

**问：AD DS Health 代理是否可以在 Server Core 上运行？**  
是的。 安装代理后，可以使用以下 PowerShell cmdlet 完成注册过程： 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**问：AADConnect 是否支持从两个域同步到 Azure AD？**</br>
是的。支持此方案。 请参阅[多个域](active-directory-aadconnect-multiple-domains.md)
 
**问：是否可对 Azure AD Connect 中的同一个 Active Directory 域使用多个连接器？**</br> 否，不支持对同一个 AD 域使用多个连接器。 

**问：是否可将 Azure AD Connect 数据库从本地数据库移到远程 SQL Server？**</br> 是的，以下步骤提供了此操作的一般指导。  我们目前正在制作更详细的文档，很快就会发布。


   1. 备份 LocalDB“ADSync”数据库。最简单的方法就是使用 Azure AD Connect 所在的计算机上安装的 SQL Server Management Studio。 连接到“(localdb)\.\ADSync”，然后备份 ADSync 数据库
   2. 将“ADSync”数据库还原到远程 SQL 实例
   3. 针对现有的[远程 SQL 数据库](active-directory-aadconnect-existing-database.md)安装 Azure AD Connect。链接显示改用本地 SQL 数据库时需要执行的步骤。 如果改用远程 SQL 数据库，则在此过程的步骤 5 中，还需要输入用于运行 Windows 同步服务的现有服务帐户。 下面描述了此同步引擎服务帐户：</br></br>
   **使用现有的服务帐户** - 默认情况下，Azure AD Connect 为要使用的同步服务使用虚拟服务帐户。 如果使用远程 SQL 服务器或使用需要身份验证的代理，则需使用托管服务帐户，或者使用域中的服务帐户并知道密码。 在这些情况下，请输入要使用的帐户。 确保运行安装的用户是 SQL 中的 SA，以便可以创建服务帐户的登录名。 请参阅 [Azure AD Connect 帐户和权限](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account)。</br></br> 现在，在使用最新版本的情况下，可以由 SQL 管理员在带外进行数据库预配，然后由具有数据库所有者权限的 Azure AD Connect 管理员完成安装。 有关详细信息，请参阅[使用 SQL 委派的管理员权限安装 Azure AD Connect](active-directory-aadconnect-sql-delegation.md)。

为简单起见，建议安装 Azure AD Connect 的用户是 SQL 中的 SA。 （但在最新的版本中，现在也可按[此处](active-directory-aadconnect-sql-delegation.md)所述使用委托的 SQL 管理员。）

## <a name="network"></a>网络
**问：我的防火墙、网络设备或其他软硬件会限制在网络上打开连接的最长时间。使用 Azure AD Connect 时，客户端超时阈值应设为多少？**  
所有网络软件、物理设备或其他软硬件限制最长连接时间的阈值应该至少为 5 分钟 (300 秒)，使装有 Azure AD Connect 客户端的服务器能够与 Azure Active Directory 连接。 此项建议同样适用于以前发布的所有 Microsoft 标识同步工具。

**问：是否支持 SLD（单一标签域）？**  
Azure AD Connect 不支持使用 SLD 的本地林/域。

**问：是否支持具有非连续 AD 域的林？**  
Azure AD Connect 不支持包含非连续命名空间的本地林。

**问：是否支持包含句点的 NetBios 名称？**  
Azure AD Connect 不支持 NetBios 名称包含句点“.”的本地林/域。

**问：是否支持纯 IPv6 环境？**  
Azure AD Connect 不支持纯 IPv6 环境。

## <a name="federation"></a>联合
**问：如果我收到一封电子邮件，要求我续订 Office 365 证书，我该怎么办？**  
请参考[续订证书](active-directory-aadconnect-o365-certs.md)文档中所述的关于如何续订证书的指南。

**问：我为 O365 信赖方设置了“自动更新信赖方”。当我的令牌签名证书自动滚动时，我是否需要采取任何措施？**  
请参考[续订证书](active-directory-aadconnect-o365-certs.md)一文中所述的指南。

## <a name="environment"></a>环境
**问：安装 Azure AD Connect 之后，是否支持重命名服务器？**  
否。 更改服务器名称将导致同步引擎无法连接到 SQL 数据库，并且服务将无法启动。

## <a name="identity-data"></a>标识数据
**问：Azure AD 中的 UPN (userPrincipalName) 属性与本地 UPN 不匹配，这是为什么？**  
请参阅以下文章：

- [Office 365、Azure 或 Intune 中的用户名与本地 UPN 或备用登录 ID 不匹配](https://support.microsoft.com/kb/2523192)
- [在将用户帐户的 UPN 更改为使用不同的联合域后，Azure Active Directory 同步工具未同步更改](https://support.microsoft.com/kb/2669550)

还可以根据 [Azure AD Connect 同步服务功能](active-directory-aadconnectsyncservice-features.md)中所述配置 Azure AD，以允许同步引擎更新 userPrincipalName。

**问：是否支持本地 AD 组/联系人对象与现有 Azure AD 组/联系人对象的软匹配？**  
是，这种软匹配取决于 proxyAddress。  未启用邮件的组不支持软匹配。

**问：是否支持手动设置现有 Azure AD 组/联系人对象的 ImmutableId 属性，以将其硬匹配到本地 AD 组/联系人对象？**  
否，目前不支持在现有的 Azure AD 组/联系人对象中手动设置 ImmutableId 属性，以便对其进行硬匹配。

## <a name="custom-configuration"></a>自定义配置
**问：在哪里可以找到 Azure AD Connect 的 PowerShell cmdlet 介绍？**  
仅支持客户使用本站点上介绍的 cmdlet，而不支持使用 Azure AD Connect 中的其他 PowerShell cmdlet。

**问：我是否可以使用 *Synchronization Service Manager* 中的“服务器导出/服务器导入”在服务器之间移动配置？**  
否。 此选项不会检索所有配置设置，因此不应使用。 应该改用向导在第二台服务器上创建基础配置，并使用同步规则编辑器生成 PowerShell 脚本，如此即可在服务器之间移动任何自定义规则。 请参阅[交叉迁移](active-directory-aadconnect-upgrade-previous-version.md#swing-migration)。

**问：是否可以为 Azure 登录页缓存密码，这是否会因为它包含一个具有 autocomplete = "false" 属性的密码输入元素而被阻止？**</br>
目前不支持修改密码输入字段的 HTML 属性，包括 autocomplete 标记。 我们目前正在开发一种功能，它将允许使用自定义 Javascript 向密码字段添加任何属性。

**问：Azure 登录页上显示之前已成功登录的用户的用户名。此行为是否可以关闭？**</br>
目前不支持修改密码输入字段的 HTML 属性，包括 autocomplete 标记。 我们目前正在开发一种功能，它将允许使用自定义 Javascript 向密码字段添加任何属性。

**问：是否有方法来阻止并发会话？**</br>
否。

## <a name="troubleshooting"></a>故障排除
**问：如何获取有关 Azure AD Connect 的帮助？**

[搜索 Microsoft 知识库 (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- 在 Microsoft 知识库 (KB) 中搜索有关 Azure AD Connect 支持的常见故障维修服务问题的技术解决方案。

[Azure Active Directory 论坛](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- 单击 [此处](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)，在社区中搜索和浏览技术问题与答案，或提出自己的问题。



<!--Update_Description: wording update-->