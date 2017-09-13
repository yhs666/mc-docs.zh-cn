---
title: "如何将操作添加到 Azure API 管理中的 API | Azure"
description: "了解如何将操作添加到 Azure API 管理中的 API。"
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 12/15/2016
ms.author: v-yiso
ms.date: 
ms.openlocfilehash: 2cbddd6f993c56d57c956b8d42c942367e419779
ms.sourcegitcommit: 81c9ff71879a72bc6ff58017867b3eaeb1ba7323
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2017
---
# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>如何将操作添加到 Azure API 管理中的 API
必须先添加操作，然后才能使用 API 管理中的 API。 本指南演示了如何将不同类型的操作添加并配置到 API 管理中的 API。

## <a name="add-operation"> </a>添加操作
可以在发布者门户中将操作添加并配置到 API。 若要访问发布者门户，请在 API 管理服务的 Azure 门户中单击“发布者门户”。

![发布者门户][api-management-management-console]

> 如果尚未创建 API 管理服务实例，请参阅 [Azure API 管理入门][Get started with Azure API Management]教程中的[创建 API 管理服务实例][Create an API Management service instance]。
> 
> 

在发布者门户中选择所需的 API，并选择“操作”选项卡。 

![操作][api-management-operations]

单击“添加操作”来添加新操作。 将显示“新建操作”，并且默认情况下将选择“签名”选项卡。

![添加操作][api-management-add-operation]

通过从下拉列表中选择来指定“HTTP 谓词”。

![HTTP 方法][api-management-http-method]

<a name="url-template"></a>

通过键入由一个或多个 URL 路径段和零个或多个查询字符串参数组成的 URL 片段来定义 URL 模板。 URL 模板附加到 API 的基础 URL，标识单个 HTTP 操作。 它可能包含一个或多个由大括号标识的命名变量部分。 这些变量部分称为模板参数，当请求由 API 管理平台进行处理时，它们是从请求的 URL 中提取的动态分配值。

> URL 模板可以包含通配符模式。 例如，指定 `/*` 会将该 HTTP 方法的所有请求都转发到后端服务。

![URL 模板][api-management-url-template]

<a name="rewrite-url-template"></a>

如果需要，请指定“重写 URL 模板”。 这样，可以在前端使用标准 URL 模板处理传入的请求，同时可以根据重写模板通过转换后的 URL 来调用后端。 应当在重写模板中使用来自 URL 模板的模板参数。 下面的示例展示了如何使用 URL 模板将前面示例中编码为 web 服务中的路径段的内容类型提供为通过 API 管理平台发布的 API 中的查询参数。

![URL 模板重写][api-management-url-template-rewrite]

操作的调用方将使用格式 `/customers?customerid=ALFKI`，调用后端服务时这会映射到 `/Customers('ALFKI')`。

“显示名称”和“说明”提供操作的说明，并用于在开发人员门户中向使用此 API 的开发人员提供文档。

![说明][api-management-description]

可以在“说明”文本框中将操作说明指定为纯文本或 HTML。

## <a name="operation-caching"> </a>操作缓存
响应缓存可降低 API 使用者感受到的延迟，降低带宽消耗并减少实现了 API 的 HTTP web 服务上的负载。 

若要轻松快速地启用操作缓存，请选择“缓存”选项卡并选中“启用”复选框。

![缓存][api-management-caching-tab]

“持续时间”指定将操作响应保留在缓存中的时间段。 默认值为 3600 秒或 1 小时。

缓存键用来区分响应，以便与每个不同缓存键对应的响应会获得自己单独的缓存值。 （可选）分别在“随查询字符串参数变化”和“随标头变化”文本框中输入在计算缓存键值时要使用的特定查询字符串参数和/或 HTTP 标头。 如果未指定，则会使用完整的请求 URL 和以下 HTTP 标头值来生成缓存密钥：**Accept** 和 **Accept-Charset**

> 有关缓存和缓存策略的详细信息，请参阅[如何在 Azure API 管理中缓存操作结果][How to cache operation results in Azure API Management]。
> 
> 

## <a name="request-parameters"> </a>请求参数
操作参数是在“参数”选项卡上管理的。在“签名”选项卡上的“URL 模板”中指定的参数会自动添加，并且只能在编辑 URL 模板时更改。 可以手动输入其他参数。

若要添加新的查询参数，请单击“添加查询参数”并输入以下信息：

* **名称** - 参数名称。
* **说明** - 参数的简短说明（可选）。
* **类型** - 参数类型，在下拉菜单中选择。
* **值** - 可以分配给此参数的值。 其中一个值可被标记为默认（可选）。
* **必需** - 通过选中复选框让参数成为必需的。 

![请求参数][api-management-request-parameters]

## <a name="request-body"> </a>请求正文
如果允许该操作（例如 PUT、 POST）并且需要正文，则可以采用所有受支持的表示形式格式（例如 json、XML）提供一个示例。 

> 请求正文仅用于文档目的，并且不对其进行验证。
> 
> 

若要输入请求正文，请切换到“正文”选项卡。

单击“添加表示形式”，开始键入所需的内容类型名称（例如 application/json），在下拉列表中选择它，并按所选格式将所需的请求正文示例粘贴到文本框中。 

![请求正文][api-management-request-body]

除了表示形式，还可在“说明”文本框中指定一个可选文本说明。

## <a name="responses"> </a>响应
为操作可能生成的所有状态代码提供相应示例是不错的做法。 每个状态代码可能有多个响应正文示例，分别针对每种支持的内容类型。 

若要添加一个响应，请单击“添加”并开始键入所需的状态代码。 在此示例中，状态代码是 **200 OK**。 代码显示在下拉列表中后，选择它，创建响应代码并将其添加到操作。

![响应代码][api-management-response-code]

单击“添加表示形式”，开始键入所需的内容类型名称（例如 application/json），并在下拉菜单中选择它。

![正文内容类型][api-management-response-body-content-type]

将响应正文示例按所选格式粘贴到文本框中。 

![响应正文][api-management-response-body]

如果需要，请将可选说明添加到“说明”文本框。

配置该操作后，单击“保存”。

## <a name="next-steps"> </a>后续步骤
将操作添加到 API 后，下一步是将该 API 与产品相关联并将其发布，以便开发人员可调用其操作。

* [如何创建和发布产品][How to create and publish a product]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: ./api-management-get-started.md
[Create an API Management service instance]: ./api-management-get-started.md#create-service-instance

[How to add operations to an API]: ./api-management-howto-add-operations.md
[How to create and publish a product]: ./api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: ./api-management-howto-cache.md
