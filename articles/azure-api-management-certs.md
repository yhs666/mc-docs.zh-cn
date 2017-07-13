---
title: "上传 Azure Management API 证书 | Azure Microsoft"
description: "了解如何为 Azure 经典门户上传 Management API 证书。"
services: cloud-services
documentationCenter: .net
authors: Thraka
manager: timlt
editor: 
ms.service: na
ms.topic: article
origin.date: 04/18/2016
ms.date: 06/13/2016
ms.author: v-junlch
ms.openlocfilehash: d7f03e0dabcb449bde87560835fe1739ed7c859d
ms.sourcegitcommit: 033f4f0e41d31d256b67fc623f12f79ab791191e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# 上传 Azure Management API 管理证书
<a id="upload-an-azure-management-api-management-certificate" class="xliff"></a>

管理证书允许你使用 Azure 提供的服务管理 API 进行身份验证。 许多程序和工具（如 Visual Studio 或 Azure SDK）将使用这些证书来自动配置和部署各种 Azure 服务。 **这仅适用于 Azure 经典门户**。 

>[!WARNING]
> 请注意！ 这些类型的证书允许使用它们进行身份验证的任何人管理与其相关联的订阅。 

有关 Azure 证书（包括创建自签名证书）的详细信息可在你需要时向你[提供](cloud-services/cloud-services-certs-create.md#what-are-management-certificates)。

还可以使用 [Azure Active Directory](/services/active-directory/) 对客户端代码进行身份验证，以用于自动化目的。

## 上传管理证书
<a id="upload-a-management-certificate" class="xliff"></a>

创建管理证书（仅包含公钥的 .cer 文件）后，可以将其上传到门户。 当该证书在门户中可用时，任何拥有匹配证书（私钥）的人都可以通过 Management API 连接并访问关联的订阅的资源。

1. 登录到 [Azure 管理门户](http://manage.windowsazure.cn)。
2. 单击门户左侧的“设置”（可能需要向下滚动）。 

    ![设置](./media/azure-api-management-certs/settings.png)

3. 单击“管理证书”选项卡。

    ![设置](./media/azure-api-management-certs/certificates-tab.png)

4. 单击“上传”按钮  。

    ![设置](./media/azure-api-management-certs/upload.png)

5. 填写对话框信息并单击完成“复选标记”。

    ![设置](./media/azure-api-management-certs/upload-dialog.png)

## 后续步骤
<a id="next-steps" class="xliff"></a>

现在已将管理证书与订阅关联，（在本地安装匹配的证书后）可以编程的方式连接到[服务管理 REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) 并自动执行各种也与该订阅关联的 Azure 资源。