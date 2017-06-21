---
title: "Visual Studio WebApi 项目中的 Azure AD 入门 | Azure"
description: "通过 Visual Studio 连接服务连接到或创建 Azure AD 之后，如何在 WebApi 项目中开始使用 Azure Active Directory"
services: active-directory
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
origin.date: 03/19/2017
ms.date: 04/17/2017
ms.author: v-junlch
translationtype: Human Translation
ms.sourcegitcommit: 7cc8d7b9c616d399509cd9dbdd155b0e9a7987a8
ms.openlocfilehash: cd76e62940f8ab6aeaa55164b3edddf3facf662a
ms.lasthandoff: 04/07/2017

---

# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a>开始使用 Azure Active Directory 和 Visual Studio 连接服务（WebApi 项目）
> [!div class="op_single_selector"]
>- [入门](./vs-active-directory-webapi-getting-started.md)
>- [发生了什么情况](./vs-active-directory-webapi-what-happened.md)

## <a name="requiring-authentication-to-access-controllers"></a>访问控制器需要身份验证
你项目中的所有控制器均带有 **Authorize** 属性。 此属性要求用户先进行身份验证，然后才能访问由这些控制器定义的 API。 若要允许匿名访问控制器，请从控制器删除此属性。 如果您想要更详细地设置这些权限，请将该属性应用到需要身份验证的每个方法，而不是将它应用到控制器类。

## <a name="next-steps"></a>后续步骤
[详细了解 Azure Active Directory](https://www.azure.cn/home/features/identity/)
