---
title: 教程 - 注册应用程序 - Azure Active Directory B2C
description: 了解如何使用 Azure 门户在 Azure Active Directory B2C 中注册 Web 应用程序。
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
origin.date: 10/16/2019
ms.date: 11/22/2019
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: a4f2552f28fc85c84c5a212229ed47fae508acee
ms.sourcegitcommit: e74e8aabc1cbd8a43e462f88d07b041e9c4f31eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2019
ms.locfileid: "74461608"
---
# <a name="tutorial-register-an-application-in-azure-active-directory-b2c"></a>教程：在 Azure Active Directory B2C 中注册应用程序

必须在你管理的租户中注册应用程序，然后这些[应用程序](active-directory-b2c-apps.md)才能与 Azure Active Directory B2C (Azure AD B2C) 交互。 本教程介绍如何使用 Azure 门户注册 Web 应用程序。

在本文中，学习如何：

> [!div class="checklist"]
> * 注册 Web 应用程序
> * 创建客户端机密

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="prerequisites"></a>先决条件

如果尚未创建自己的 [Azure AD B2C 租户](tutorial-create-tenant.md)，请立即创建一个。 可以使用现有的 Azure AD B2C 租户。

## <a name="register-a-web-application"></a>注册 Web 应用程序

若要在 Azure AD B2C 租户中注册应用程序，可以使用当前“应用程序”体验  。 

#### <a name="applicationstabapplications"></a>[应用程序](#tab/applications/)

1. 请确保使用包含 Azure AD B2C 租户的目录，方法是选择顶部菜单中的“目录 + 订阅”筛选器，然后选择包含租户的目录  。
1. 选择 Azure 门户左上角的“所有服务”，然后搜索并选择“Azure AD B2C”   。
1. 选择“应用程序”，然后选择“添加”   。
1. 输入应用程序的名称。 例如，“webapp1”  。
1. 对于“包括 Web 应用/Web API”和“允许隐式流”，请选择“是”。   
1. 对于“回复 URL”，请输入 Azure AD B2C 要将应用程序请求的任何令牌返回到的终结点  。 例如，可将其设置为在 `https://localhost:44316` 上本地侦听。 如果尚不知道端口号，可以先输入占位符值，以后再进行更改。

    出于测试目的（如本教程），可以将其设置为 `https://jwt.ms`，以便显示令牌的内容以供检查。 对于本教程，请将**回复 URL** 设置为 `https://jwt.ms`。

    以下限制适用于回复 URL：

    * 回复 URL 必须以方案 `https` 开头。
    * 回复 URL 区分大小写。 其大小写必须与正在运行的应用程序的 URL 路径的大小写匹配。 例如，如果应用程序在其路径中包括 `.../abc/response-oidc`，请不要在回复 URL 中指定 `.../ABC/response-oidc`。 由于 Web 浏览器将路径视为区分大小写，因此如果重定向到大小写不匹配的 `.../ABC/response-oidc` URL，则可能会排除与 `.../abc/response-oidc` 关联的 Cookie。

1. 单击“创建”  以完成应用程序注册。

## <a name="create-a-client-secret"></a>创建客户端机密

如果应用程序将代码交换为令牌，则需要创建应用程序机密。

#### <a name="applicationstabapplications"></a>[应用程序](#tab/applications/)

1. 在“Azure AD B2C - 应用程序”  页中，选择已创建的应用程序，例如 webapp1  。
1. 选择“密钥”，然后选择“生成密钥”。  
1. 选择“保存”以查看密钥。  记下“应用密钥”值  。 在应用程序的代码中将此值用作应用程序机密。

## <a name="next-steps"></a>后续步骤

本文介绍了如何执行以下操作：

> [!div class="checklist"]
> * 注册 Web 应用程序
> * 创建客户端机密

接下来，了解如何创建用户流以使用户能够注册、登录和管理其个人资料。

> [!div class="nextstepaction"]
> [在 Azure Active Directory B2C 中创建用户流](tutorial-create-user-flows.md)

<!-- Update_Description: wording update -->