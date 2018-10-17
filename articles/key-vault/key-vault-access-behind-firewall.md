---
title: 访问防火墙后面的 Key Vault
description: 了解如何从防火墙后面的应用程序访问 Azure Key Vault
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 50d21774-2ee1-4212-8995-570c9de603c5
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 01/07/2017
ms.date: 10/22/2018
ms.author: v-biyu
ms.openlocfilehash: 9957928f7a8190b55b08453b7f3d0a4d9d64f0d4
ms.sourcegitcommit: 2fdf25eb4b978855ff2832bcdcca093c141be261
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "49120603"
---
# <a name="access-azure-key-vault-behind-a-firewall"></a>访问防火墙后面的 Azure Key Vault
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-ports-hosts-or-ip-addresses-should-i-open-to-enable-access-to-a-key-vault"></a>问：我的密钥保管库客户端应用程序需位于防火墙之后。 我应该打开什么样的端口、主机或 IP 地址来启用密钥保管库访问权限？
若要访问密钥保管库，密钥保管库客户端应用程序必须访问多个终结点才能使用各种功能：

* 通过 Azure Active Directory (Azure AD) 进行身份验证。
* 管理 Azure Key Vault。 这包括通过 Azure Resource Manager 创建、读取、更新、删除和设置访问策略。
* 通过密钥保管库特定的终结点（例如 [https://yourvaultname.vault.azure.cn](https://yourvaultname.vault.azure.cn)），访问和管理密钥保管库本身存储的对象（密钥和机密）。  

根据配置和环境，会有一些变化。   

## <a name="ports"></a>端口
针对所有 3 项功能（身份验证、管理和数据平面访问）的所有密钥保管库流量都会通过 HTTPS（端口 443）传递。 但是，对于 CRL，偶尔会有 HTTP（端口 80）流量。 支持 OCSP 的客户端不应到达 CRL，但有时可能会到达 [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl)。  

## <a name="authentication"></a>身份验证
密钥保管库客户端应用程序需要访问 Azure Active Directory 终结点进行身份验证。 使用的终结点取决于 Azure AD 租户配置、主体类型（用户主体或服务主体）以及帐户类型（如 Microsoft 帐户或者工作或学校帐户）。  

| 主体类型 | 终结点：端口 |
| --- | --- |
| 使用 Microsoft 帐户的用户<br> （例如：user@hotmail.com） |**全球：**<br> login.microsoftonline.com:443<br><br> **Azure China：**<br> login.chinacloudapi.cn:443<br><br>**Azure US Government：**<br> login.microsoftonline.us:443<br><br>**Azure Germany：**<br> login.microsoftonline.de:443<br><br> 和 <br>login.live.com:443 |
| 使用 Azure AD 的工作或学校帐户的用户或服务主体（如 user@contoso.com） |**全球：**<br> login.microsoftonline.com:443<br><br> **Azure China：**<br> login.chinacloudapi.cn:443<br><br>**Azure US Government：**<br> login.microsoftonline.us:443<br><br>**Azure Germany：**<br> login.microsoftonline.de:443 |
| 使用工作或学校帐户，以及 Active Directory 联合身份验证服务 (AD FS) 或其他联合终结点的用户或服务主体（如 user@contoso.com） |适用于工作或学校帐户的所有终结点，以及 AD FS 或其他联合终结点 |

还有其他可能的复杂情况。 有关其他信息，请参阅 [Azure Active Directory 身份验证流](/active-directory/develop/active-directory-authentication-scenarios)、[将应用程序与 Azure Active Directory 集成](/active-directory/develop/active-directory-how-to-integrate)和 [Active Directory 身份验证协议](/active-directory/develop/active-directory-developers-guide)。  

## <a name="key-vault-management"></a>密钥保管库管理
对于 Key Vault 管理（CRUD 和设置访问策略），Key Vault 客户端应用程序需要访问 Azure Resource Manager 终结点。  

| 操作类型 | 终结点：端口 |
| --- | --- |
| 通过 Azure Resource Manager 进行的<br> （通过 Azure 资源管理器执行） |**全球：**<br> management.azure.com:443<br><br> **Azure China：**<br> management.chinacloudapi.cn:443<br><br> **Azure US Government：**<br> management.usgovcloudapi.net:443<br><br> **Azure Germany：**<br> management.microsoftazure.de:443 |
| Azure Active Directory 图形 API |**全球：**<br> graph.chinacloudapi.cn:443<br><br> **Azure China：**<br> graph.chinacloudapi.cn:443<br><br> **Azure US Government：**<br> graph.chinacloudapi.cn:443<br><br> **Azure Germany：**<br> graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>密钥保管库操作
对于所有密钥保管库对象（密钥和密码）管理和加密操作，密钥保管库客户端需要访问密钥保管库终结点。 终结点 DNS 后缀因密钥保管库位置而异。 密钥保管库终结点的格式是 vault-name.region-specific-dns-suffix，如下表所示。  

| 操作类型 | 终结点：端口 |
| --- | --- |
| 操作包括对密钥的加密操作；创建、读取、更新和删除密钥和密码；设置或获取密钥保管库对象（密钥或密码）上的标记和其他属性 |**全局：**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure China：**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure US Government：**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Germany：**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP 地址范围
Key Vault 服务使用其他 Azure 资源，例如 PaaS 基础结构。 因此，不可能提供 Key Vault 服务终结点在任意特定时间会有的特定 IP 地址范围。 如果防火墙仅支持 IP 地址范围，请参阅 [Azure Datacenter IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653) （Azure 数据中心 IP 范围）文档。 身份验证和标识 (Azure Active Directory) 是一项全球性服务，可能会故障转移到其他区域或移动流量，恕不另行通知。 在这种情况下，[身份验证和标识 IP 地址](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity_ip)中列出的所有 IP 范围都应添加到防火墙中。

## <a name="next-steps"></a>后续步骤
如果在 Key Vault 方面有任何问题，请访问 [Azure Key Vault 论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)。

<!--Update_Description: wording update -->
