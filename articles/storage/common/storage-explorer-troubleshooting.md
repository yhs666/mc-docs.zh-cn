---
title: Azure 存储资源管理器故障排除指南 | Azure
description: Azure 两项调试功能的概述
services: virtual-machines
documentationcenter: ''
author: forester123
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
origin.date: 09/08/2017
ms.date: 10/30/2017
ms.author: v-johch
ms.openlocfilehash: a48c02680717ba67681db8a2203bd51cae2b19f2
ms.sourcegitcommit: ad7accbbd1bc7ce0aeb2b58ce9013b7cafa4668b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2018
ms.locfileid: "29870384"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure 存储资源管理器故障排除指南

Azure 存储资源管理器（预览版）是一款独立应用，可用于在 Windows、macOS 和 Linux 上轻松处理 Azure 存储数据。 应用可连接到托管在 Azure、National Clouds 和 Azure Stack 上的存储帐户。

本指南汇总了存储资源管理器中常见问题的解决方案。

## <a name="sign-in-issues"></a>登录问题

仅支持 Azure Active Directory (AAD) 帐户。 如果使用 ADFS 帐户，预计将无法正常登录存储资源管理器。 在继续之前，尝试重启应用程序，并检查是否已解决问题。

### <a name="error-self-signed-certificate-in-certificate-chain"></a>错误：证书链中的自签名证书

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

如果通过上述步骤无法找到任何自签名证书，请通过反馈工具联系我们以获取更多帮助。

### <a name="unable-to-retrieve-subscriptions"></a>无法检索订阅

如果成功登录后无法检索订阅，请按照下列步骤操作，解决此问题：

- 通过登录 Azure 门户验证帐户是否有权访问该订阅。

- 请确保使用正确的环境登录（Azure、Azure 中国、Azure 德国、Azure 美国政府或自定义环境/Azure Stack）。

- 如果使用了代理，请确保已正确配置存储资源管理器代理。

- 尝试删除并重新添加帐户。

- 尝试从根目录（即 C:\Users\ContosoUser）删除以下文件，然后重新添加帐户：

    - .adalcache

    - .devaccounts

    - .extaccounts

- 登录过程中出现任何错误消息时，请监视开发人员工具控制台（按 F12）：

![开发人员工具](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-to-see-the-authentication-page"></a>无法查看身份验证页

如果无法查看身份验证页，请按照下列步骤操作，解决此问题：

- 登录页的加载需要一段时间，具体取决于连接速度。请至少等待一分钟，然后再关闭身份验证对话框。

- 如果使用了代理，请确保已正确配置存储资源管理器代理。

- 按 F12 键查看开发人员控制台。 监视开发人员控制台的响应，并查看是否可以找到有关身份验证不起作用的原因的任何线索。

### <a name="cannot-remove-account"></a>无法删除帐户

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
>  删除上述文件后，需要重新登录到你的帐户。

## <a name="proxy-issues"></a>代理问题

首先，请确保输入的以下信息都是正确的：

- 代理 URL 和端口号

- 用户名和密码（如果代理需要）

### <a name="common-solutions"></a>常见解决方法

如果仍遇到问题，请按照下列步骤操作，解决这些问题：

- 如果在不使用代理时可以连接到 Internet，请确认未启用代理设置时存储资源管理器可以正常工作。 如果出现这种情况，可能是代理设置有问题。 与代理服务器管理员合作，找到问题。

- 确认使用代理服务器的其他应用程序按预期运行。

- 确认可以使用 Web 浏览器连接到 Azure 门户

- 确认可以收到来自服务终结点的响应。 在浏览器中输入其中一个终结点 URL。 如果可以连接，应收到 InvalidQueryParameterValue 或类似的 XML 响应。

- 如果其他人也在通过该代理服务器使用存储资源管理器，请确认他们可以连接。 如果他们可以连接，请与代理服务器管理员联系。

### <a name="tools-for-diagnosing-issues"></a>诊断问题的工具

如果有 Fiddler for Windows 等网络服务工具，则能够按以下方式诊断问题：

- 如果必须通过代理工作，则必须将网络服务工具配置为通过代理进行连接。

- 检查网络服务工具使用的端口号。

- 输入存储资源管理器中代理设置的本地主机 URL 和网络服务工具的端口号。 如果正确完成此操作，网络服务工具将开始记录存储资源管理区向管理和服务终结点发出的网络请求。 例如，在浏览器中为 blob 终结点输入 https://cawablobgrs.blob.core.chinacloudapi.cn/，将收到以下类似响应，这表明虽然不能访问该资源，但该资源存在。

![代码示例](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>与代理服务器管理员联系

如果代理设置正确，则必须与代理服务器管理员联系，并

- 确保代理不会阻止到 Azure 管理或资源终结点的流量。

- 验证代理服务器使用的身份验证协议。 存储资源管理器当前不支持 NTLM 代理。

## <a name="unable-to-retrieve-children-error-message"></a>“无法检索子级”错误消息

如果通过代理连接到 Azure，请确认代理设置正确无误。 如果已获得对订阅或帐户所有者的资源的访问权限，请确认具有读取或列出该资源的权限。

### <a name="issues-with-sas-url"></a>SAS URL 的问题
如果使用 SAS URL 连接到服务时遇到此错误：

- 请确认 URL 提供了读取或列出资源所需的权限。

- 请确认 URL 未过期。

- 如果 SAS URL 基于访问策略，请确认访问策略尚未吊销。

如果意外附加了无效的 SAS URL，并且无法分离，请执行以下步骤：
1.  运行存储资源管理器时，按 F12 打开开发人员工具窗口。
2.  单击“应用程序”选项卡，然后单击左侧树中的“本地存储”> file://。
3.  查找与有问题的 SAS URI 服务类型关联的键。 例如，如果用于 blob 容器的 SAS URI 错误，请查找名为“StorageExplorer_AddStorageServiceSAS_v1_blob”的键。
4.  键的值应为 JSON 数组。 查找与错误 URI 关联的对象，并将其删除。
5.  按 Ctrl+R 重新加载存储资源管理器。

## <a name="linux-dependencies"></a>Linux 依赖项

## <a name="next-steps"></a>后续步骤

如果以上解决方案均不起作用，可以通过反馈工具提交问题，并提供电子邮件和尽可能多的问题详细信息，以便我们联系你解决问题。

为此，请单击“帮助”菜单，然后单击“发送反馈”。

![反馈](./media/storage-explorer-troubleshooting/4022503_en_1.png)
<!--Update_Description: add note that ADFS account is not supported-->
