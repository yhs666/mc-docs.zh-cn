---
title: Azure API 管理开发人员门户概述 - Azure API 管理 | Microsoft Docs
description: 了解 API 管理中的开发人员门户。
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
origin.date: 11/04/2019
ms.date: 12/09/2019
ms.author: v-yiso
ms.openlocfilehash: b10d4ffc77bd9c81c83f32fcbe7030c530025875
ms.sourcegitcommit: 298eab5107c5fb09bf13351efeafab5b18373901
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2019
ms.locfileid: "74657624"
---
# <a name="azure-api-management-developer-portal-overview"></a>Azure API 管理开发人员门户概述

开发人员门户是一个自动生成的、完全可自定义的网站，其中包含 API 的文档。 API 使用者可在其中找到 API、了解 API 的用法、请求访问权限以及试用这些 API。

本文介绍了 API 管理中开发人员门户的自承载版本与托管版本之间的差异。 此外介绍此门户的体系结构，并提供常见问题的解答。

> [!WARNING]
> 目前正在推出适用于 API 管理服务的新的开发人员门户。
> 如果你的服务是新创建的或开发人员层服务，则你应该已有最新版本。 否则，你可能会遇到问题（例如，使用发布功能时）。 该功能推出预计将于 2019 年 11 月 22 日（星期五）前完成。
>
> [了解如何从开发人员门户预览版迁移到正式版](#preview-to-ga)。

![API 管理开发人员门户](media/api-management-howto-developer-portal/cover.png)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="managed-vs-self-hosted"></a> 托管版本和自承载版本

可通过两种方式构建开发人员门户：

- **托管版本** - 通过编辑和自定义 API 管理实例中内置的、可通过 URL `<your-api-management-instance-name>.developer.azure-api.net` 访问的门户。 请参阅[此文档](api-management-howto-developer-portal-customize.md)了解如何访问和自定义托管门户。
- **自承载版本** - 通过在 API 管理实例外部部署和自承载门户。 使用此方法可以编辑门户的基代码并扩展所提供的核心功能。 还需要自行将门户升级到最新版本。 有关详细信息和说明，请参阅[包含门户源代码的 GitHub 存储库][1]。 [托管版本的教程](api-management-howto-developer-portal-customize.md)全面介绍了门户的管理面板（自承载版本中也有此面板）。

## <a name="portal-architectural-concepts"></a>门户体系结构概念

门户组件在逻辑上可以划分为两个类别：代码和内容。  

代码在 [GitHub 存储库][1]中维护，包括： 

- 小组件 - 呈现视觉元素并组合了 HTML、JavaScript、样式功能、设置和内容映射。 例如，图像、文本段落、表单、API 列表等。
- 样式定义 - 指定如何设置小组件的样式
- 引擎 - 基于门户内容生成静态网页，是以 JavaScript 编写的
- 视觉编辑器 - 用于浏览器内部的自定义和创作体验

内容划分为两个子类别：门户内容和 API 管理内容。   

门户内容特定于门户，包括： 

- 页面 - 例如登陆页、API 教程和博客文章
- 媒体 - 图像、动画和其他基于文件的内容
- 布局 - 与 URL 匹配的模板，定义页面显示方式
- 样式 - 样式定义值，例如字体、颜色和边框
- 设置 - 网站图标、网站元数据等配置

门户内容（媒体除外）以 JSON 文档的形式表示。 

API 管理内容包括 API、操作、产品和订阅等实体。 

门户基于 [Paperbits 框架](https://paperbits.io/)的改编分叉。 原始 Paperbits 功能已经过扩展，提供特定于 API 管理的小组件（例如 API 列表、产品列表）以及与 API 管理服务之间的连接器，可以保存和检索内容。

## <a name="faq"></a> 常见问题

本部分解答有关新开发人员门户的一般性常见问题。 有关自承载版本的特定问题，请参阅 [GitHub 存储库的 wiki 部分](https://github.com/Azure/api-management-developer-portal/wiki)。

### <a name="a-idpreview-to-ga-how-can-i-migrate-from-the-preview-version-of-the-portal"></a><a id="preview-to-ga"/> 如何从门户预览版迁移？

你已使用开发人员门户预览版在 API 管理服务中预配了预览内容。 为了提供更好的用户体验，默认内容已在正式版中进行重大修改。 正式版中还包括新的小组件。

如果使用的是托管版本，请通过单击“操作”菜单部分中的“重置内容”来重置门户内容。   确认此操作会删除门户的所有内容并预配新的默认内容。 门户引擎已在 API 管理服务中自动更新。

![重置门户内容](media/api-management-howto-developer-portal/reset-content.png)

如果使用的是自承载版本，请使用 GitHub 存储库中的 `scripts/cleanup.bat` 和 `scripts/generate.bat` 删除现有内容并预配新内容。 确保提前将门户代码升级到 GitHub 存储库中的最新版本。

如果不想要重置门户内容，可以考虑在整个页面中使用新的可用小组件。 现有的小组件已自动更新到最新版本。

如果门户是宣布推出正式版后预配的，则它应已具有新的默认内容。 你无需在自己的一端执行任何操作。

### <a name="how-can-i-migrate-from-the-old-developer-portal-to-the-new-developer-portal"></a>如何从旧开发人员门户迁移到新开发人员门户？

这两个门户不兼容，需要手动迁移内容。

### <a name="does-the-new-portal-have-all-the-features-of-the-old-portal"></a>新门户是否拥有旧门户的所有功能？

新开发人员门户不支持“应用程序”和“问题”。   如果你在旧门户中使用了“问题”，并需要在新门户中继续使用该功能，请在[相关的 GitHub 问题](https://github.com/Azure/api-management-developer-portal/issues/122)中留言。 

### <a name="has-the-old-portal-been-deprecated"></a>旧门户是否已弃用？

旧的开发人员和发布者门户现在属于旧版功能 - 它们只会接收安全更新。  新功能只会在新开发人员门户中实现。

旧门户弃用时，我们会另行通告。 若有问题、疑虑或意见，请提出[相关的 GitHub 问题](https://github.com/Azure/api-management-developer-portal/issues/121)。

### <a name="how-can-i-automate-portal-deployments"></a>如何自动部署门户？

不管使用的是托管版本还是自承载版本，都可以通过 REST API 以编程方式访问和管理开发人员门户的内容。

[GitHub 存储库的 Wiki 部分][2]介绍了该 API。 也可以使用该 API 在环境之间自动迁移门户内容 - 例如，从测试环境迁移到生产环境。 可以在 GitHub 上的[此文档](https://aka.ms/apimdocs/migrateportal)中详细了解此过程。

### <a name="does-the-portal-support-azure-resource-manager-templates-andor-is-it-compatible-with-api-management-devops-resource-kit"></a>门户是否支持 Azure 资源管理器模板，和/或是否与 API 管理 DevOps 资源工具包兼容？

否。

### <a name="do-i-need-to-enable-additional-vnet-connectivity-for-the-managed-portal-dependencies"></a>是否需要为托管门户依赖项启用额外的 VNET 连接？

否。

### <a name="im-getting-a-cors-error-when-using-the-interactive-console-what-should-i-do"></a>使用交互式控制台时出现 CORS 错误。 我该怎么办？

交互式控制台从浏览器发出客户端 API 请求。 在 API 中添加 [CORS 策略](/api-management/api-management-cross-domain-policies#CORS)可以解决 CORS 问题。 可以手动指定所有参数，也可以使用通配符 `*` 值。 例如：

```XML
<cors>
    <allowed-origins>
        <origin>*</origin>
    </allowed-origins>
    <allowed-methods>
        <method>GET</method>
        <method>POST</method>
        <method>PUT</method>
        <method>DELETE</method>
        <method>HEAD</method>
        <method>OPTIONS</method>
        <method>PATCH</method>
        <method>TRACE</method>
    </allowed-methods>
    <allowed-headers>
        <header>*</header>
    </allowed-headers>
    <expose-headers>
        <header>*</header>
    </expose-headers>
</cors>
```

## <a name="next-steps"></a>后续步骤

详细了解新开发人员门户：

- [访问和自定义托管开发人员门户](api-management-howto-developer-portal-customize.md)
- [设置自承载版本的门户][2]

浏览其他资源：

- [包含源代码的 GitHub 存储库][1]
- [项目的公开路线图][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects