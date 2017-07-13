---
title: "Azure 存储资源管理器故障排除指南 | Microsoft Docs"
description: "Azure 两项调试功能的概述"
services: virtual-machines
documentationcenter: 
author: forester123
manager: digimobile
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 05/18/2017
ms.date: 06/26/2017
ms.author: v-johch
ms.openlocfilehash: 0eed830c6d3a1cd7141c1a7ce66a87dd067fe84e
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# Azure 存储资源管理器故障排除指南
<a id="azure-storage-explorer-troubleshooting-guide" class="xliff"></a>

## 介绍
<a id="introduction" class="xliff"></a>

Azure 存储资源管理器（预览版）是一款独立应用，可用于在 Windows、macOS 和 Linux 上轻松处理 Azure 存储数据。 该应用可以连接 Azure、 Sovereign Clouds 和 Azure Stack 上托管的存储帐户。

本指南汇总了存储资源管理器中常见问题的解决方案。

## 登录问题
<a id="sign-in-issues" class="xliff"></a>

在继续之前，尝试重启应用程序，并检查是否已解决问题。

### 错误：证书链中的自签名证书
<a id="error-self-signed-certificate-in-certificate-chain" class="xliff"></a>

遇到此错误的原因有多个，其中最常见的两个原因如下所述：

1. 应用通过“透明代理”进行连接，这意味着一台服务器（例如公司的服务器）正在截取 HTTPS 流量，对其进行解密，然后使用自签名证书对其进行加密。

2. 正在运行防病毒软件等应用程序，这会将自签名的 SSL 证书注入到收到的 HTTPS 消息中。

存储资源管理器遇到以上任一问题时，将无法获知收到的 HTTPS 消息是否已被篡改。 如果有自签名证书的副本，可以让存储资源管理器信任它。 如果不确定注入证书的操作者，请执行这些步骤进行查找：

1. 安装 Open SSL

    - [Windows](https://slproweb.com/products/Win32OpenSSL.html)（任何精简版本均可）

    - Mac 和 Linux：应包含在操作系统中

2. 运行 Open SSL

    - Windows：打开安装目录，单击“/bin/”，然后双击“openssl.exe”。
    - Mac 和 Linux：从终端运行 penssl。

3. 执行 s_client -showcerts -connect microsoft.com:443

4. 查找自签名证书。 如果不确定哪个证书是自签名的，请查找使用者 ("s:") 和颁发者 ("i:") 相同的证书。

5. 找到任何自签名证书时，对于每个证书，将从“-----BEGIN CERTIFICATE-----”到“-----END CERTIFICATE-----”在内的所有内容复制并粘贴到新的 .cer 文件。

6. 打开存储资源管理器，单击“编辑” > “SSL 证书” > “导入证书”，然后使用文件选取器查找、选择并打开已创建的 .cer 文件。

如果使用上述步骤无法找到任何自签名证书，请通过反馈工具与我们联系以获取更多帮助。

### 无法检索订阅
<a id="unable-to-retrieve-subscriptions" class="xliff"></a>

如果成功登录后无法检索订阅，请按照下列步骤操作，解决此问题：

- 登录 Azure 门户，验证帐户是否有权访问订阅。

- 请确保使用正确的环境（Azure、Azure China、Azure Germany、Azure US Government 或自定义环境/Azure Stack）登录。

- 如果使用了代理，请确保已正确配置存储资源管理器代理。

- 尝试删除并重新添加帐户。

- 尝试从根目录（即 C:\Users\ContosoUser）删除以下文件，然后重新添加帐户：

    - .adalcache

    - .devaccounts

    - .extaccounts

- 登录过程中出现任何错误消息时，请监视开发人员工具控制台（按 F12）：

![开发人员工具](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### 无法查看身份验证页
<a id="unable-to-see-the-authentication-page" class="xliff"></a>

如果无法查看身份验证页，请按照下列步骤操作，解决此问题：

- 登录页的加载需要一段时间，具体取决于连接速度。请至少等待一分钟，然后再关闭身份验证对话框。

- 如果使用了代理，请确保已正确配置存储资源管理器代理。

- 按 F12 键查看开发人员控制台。 监视开发人员控制台的响应，并查看是否可以找到有关身份验证不起作用的原因的任何线索。

### 无法删除帐户
<a id="cannot-remove-account" class="xliff"></a>

如果无法删除某个帐户，或者重新验证链接不执行任何操作，请按照下列步骤操作，解决此问题：

- 尝试从根目录删除以下文件，然后重新添加帐户：

    - .adalcache

    - .devaccounts

    - .extaccounts

- 若要删除 SAS 附加的存储资源，请删除以下文件：

    - %AppData%/StorageExplorer 文件夹（对于 Windows）

    - /Users/<your_name>/Library/Applicaiton SUpport/StorageExplorer（对于 Mac）

    - ~/.config/StorageExplorer（对于 Linux）

> [!NOTE]
>  删除这些文件后，需要重新输入凭据。

## 代理问题
<a id="proxy-issues" class="xliff"></a>

首先，请确保输入的以下信息都是正确的：

- 代理 URL 和端口号

- 用户名和密码（如果代理需要）

### 常见解决方法
<a id="common-solutions" class="xliff"></a>

如果仍遇到问题，请按照下列步骤操作，解决这些问题：

- 如果在不使用代理时可以连接到 Internet，请确认未启用代理设置时存储资源管理器可以正常工作。 如果出现这种情况，可能是代理设置有问题。 与代理服务器管理员合作，找到问题。

- 确认使用代理服务器的其他应用程序按预期运行。

- 确认可以使用 Web 浏览器连接到 Azure 门户

- 确认可以收到来自服务终结点的响应。 在浏览器中输入其中一个终结点 URL。 如果可以连接，应收到 InvalidQueryParameterValue 或类似的 XML 响应。

- 如果其他人也在通过该代理服务器使用存储资源管理器，请确认他们可以连接。 如果他们可以连接，请与代理服务器管理员联系。

### 诊断问题的工具
<a id="tools-for-diagnosing-issues" class="xliff"></a>

如果有 Fiddler for Windows 等网络服务工具，则能够按以下方式诊断问题：

- 如果必须通过代理工作，则必须将网络服务工具配置为通过代理进行连接。

- 检查网络服务工具使用的端口号。

- 输入存储资源管理器中代理设置的本地主机 URL 和网络服务工具的端口号。 如果正确完成此操作，网络服务工具将开始记录存储资源管理区向管理和服务终结点发出的网络请求。 例如，在浏览器中为 blob 终结点输入 https://cawablobgrs.blob.core.windows.net/ ，将收到以下类似响应，这表明虽然不能访问该资源，但该资源存在。

![代码示例](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### 与代理服务器管理员联系
<a id="contact-proxy-server-admin" class="xliff"></a>

如果代理设置正确，则必须与代理服务器管理员联系，并

- 确保代理不会阻止到 Azure 管理或资源终结点的流量。

- 验证代理服务器使用的身份验证协议。 存储资源管理器当前不支持 NTLM 代理。

## “无法检索子级”错误消息
<a id="unable-to-retrieve-children-error-message" class="xliff"></a>

如果通过代理连接到 Azure，请确认代理设置正确无误。 如果已获得对订阅或帐户所有者的资源的访问权限，请确认具有读取或列出该资源的权限。

### SAS URL 的问题
<a id="issues-with-sas-url" class="xliff"></a>
如果使用 SAS URL 连接到服务时遇到此错误：

- 请确认 URL 提供了读取或列出资源所需的权限。

- 请确认 URL 未过期。

- 如果 SAS URL 基于访问策略，请确认访问策略尚未吊销。


