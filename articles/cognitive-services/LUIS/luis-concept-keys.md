---
title: 创作密钥和运行时密钥 - LUIS
titleSuffix: Azure Cognitive Services
description: LUIS 使用两种密钥，其中的创作密钥用于创建模型，运行时密钥用于通过用户话语查询预测终结点。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
origin.date: 09/02/2019
ms.date: 09/25/2019
ms.author: v-lingwu
ms.openlocfilehash: da9cc633043de6368cd0f041f5f6a7a50934d93c
ms.sourcegitcommit: 2f2ced6cfaca64989ad6114a6b5bc76700870c1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71329923"
---
# <a name="authoring-and-runtime-keys"></a>创作和运行时密钥


LUIS 使用两种类型的 Azure 资源，每种类型都有密钥： 
 
* [创作密钥](#programmatic-key)，用于创建意向、实体和带标签的话语，以及用于训练和发布。 准备好发布 LUIS 应用时，需要一个[用于运行时的预测终结点密钥](luis-how-to-azure-subscription.md)（分配给应用）。
* [用于运行时的预测终结点密钥](#prediction-endpoint-runtime-key)。 客户端应用程序（例如聊天机器人）需要通过此密钥访问运行时的**查询预测终结点**。 

|键|目的|认知服务 `kind`|认知服务 `type`|
|--|--|--|--|
|[创作密钥](#programmatic-key)|创作、训练、发布、测试。|`LUIS.Authoring`|`Cognitive Services`|
|[预测终结点运行时密钥](#prediction-endpoint-runtime-key)| 查询预测终结点运行时，通过用户话语来确定意向和实体。|`LUIS`|`Cognitive Services`|

LUIS 还提供[初学者密钥](luis-how-to-azure-subscription.md#starter-key)，其预测终结点配额为每月 1000 个事务。 

虽然不需同时创建这两种密钥，但这样做会带来很大方便。

请务必在想要进行发布和查询的[区域](luis-reference-regions.md#publishing-regions)创作 LUIS 应用。

<a name="programmatic-key" ></a>

## <a name="authoring-key"></a>创作密钥

创作密钥是在创建 LUIS 帐户时自动创建的免费密钥。 开始使用 LUIS 时，每个创作[区域](luis-reference-regions.md)的所有 LUIS 应用共享一个初学者密钥。 创作密钥的用途是提供管理 LUIS 应用所需的身份验证，或测试预测终结点查询。 

通过在 Azure 门户中创建创作密钥，可以为人员分配[参与者角色](#contributions-from-other-authors)，控制对创作资源的权限。 需要 Azure 订阅级别的权限才能添加参与者。 

若要找到创作密钥，请登录 [LUIS](luis-reference-regions.md#luis-website) 并单击右上方导航栏中的帐户名称，打开“帐户设置”  。

![创作密钥](./media/luis-concept-keys/authoring-key.png)

需要进行运行时查询时，请创建 Azure [LUIS 订阅](https://www.azure.cn/pricing/details/cognitive-services)。  

> [!CAUTION]
> 为了方便起见，很多示例使用[初学者密钥](#starter-prediction-endpoint-runtime-key)，因为该密钥在[配额](luis-boundaries.md#key-limits)中提供了几个免费的预测终结点调用。  

<a name="endpoint-key"></a>

## <a name="prediction-endpoint-runtime-key"></a>预测终结点运行时密钥 

需要**运行时终结点查询**时，请创建语言理解 (LUIS) 资源，然后将其分配给 LUIS 应用。 

[!INCLUDE [Azure runtime resource creation for Language Understanding and Cognitive Service resources](../../../includes/cognitive-services-luis-azure-resource-instructions.md)]

资源创建过程完成后，请为应用[分配密钥](luis-how-to-azure-subscription.md)。 

* 运行时（查询预测终结点）密钥支持的终结点命中次数配额取决于创建运行时密钥时所指定的使用计划。  请参阅[认知服务定价](https://www.azure.cn/pricing/details/cognitive-services)了解定价信息。

* 运行时密钥可用于所有 LUIS 应用或特定 LUIS 应用。 
* 请勿将运行时密钥用于创作 LUIS 应用。 

### <a name="starter-prediction-endpoint-runtime-key"></a>初学者预测终结点运行时密钥

**初学者**预测终结点密钥免费提供，包含 1000 个预测终结点查询。 使用这些查询后，应该创建你自己的用于语言理解的预测终结点资源。  

这是为你创建的特殊资源。 它没有显示在 Azure 资源列表中，因为它用作临时启动密钥。 

<a name="use-endpoint-key-in-query"></a>

### <a name="use-runtime-key-in-query"></a>在查询中使用运行时密钥
LUIS 运行时终结点接受两种样式的查询，这两种查询都使用预测终结点运行时密钥，但使用的位置不同。

用于访问运行时的终结点使用一个特定于资源所在区域的子域，该区域在下表中使用 `{region}` 表示。 

|Verb|示例 URL 和密钥位置|
|--|--|
|[GET](https://{region}.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?runtime-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`<br><br>`runtime-key` 的查询字符串值<br><br>将 `runtime-key` 的终结点查询值从创作（初学者）密钥更改为新的终结点密钥，以便使用 LUIS 终结点密钥配额率。 如果创建并分配了该密钥，但是没有更改 `runtime-key` 的终结点查询值，则不会使用终结点密钥配额。|
|[POST](https://{region}.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`<br><br> `Ocp-Apim-Subscription-Key` 的标头值<br>如果创建并分配了运行时密钥，但是没有更改 `Ocp-Apim-Subscription-Key` 的终结点查询值，则不会使用运行时密钥。|

在以前的 URL 中使用的应用 ID `df67dcdb-c37d-46af-88e1-8b97951ca1c2` 是用于[互动演示](https://www.azure.cn/zh-cn/home/features/cognitive-services/language-understanding-intelligent-service/)的公共 IoT 应用。 

## <a name="assignment-of-the-runtime-key"></a>分配运行时密钥

可以通过 [LUIS 门户](https://luis.azure.cn)或相应的 API [分配](luis-how-to-azure-subscription.md)运行时密钥。 

## <a name="key-limits"></a>密钥限制

可以按订阅为每个区域创建最多 10 个创作密钥。 

请参阅[密钥限制](luis-boundaries.md#key-limits)和 [Azure 区域](luis-reference-regions.md)。 

发布区域不同于创作区域。 请确保在对应于发布区域的创作区域中创建应用，你需要将客户端应用程序置于该发布区域中。

## <a name="key-limit-errors"></a>密钥限制错误
如果超过了每秒事务数 (TPS) 配额，则会出现 HTTP 429 错误。 如果超过了每月事务数 (TPM) 配额，则会出现 HTTP 403 错误。 

## <a name="contributions-from-other-authors"></a>其他作者的贡献



对协作者的贡献的管理取决于应用的当前状态。

**对于尚未迁移的应用**：所有协作者  都在 LUIS 门户中通过“管理 -> 协作者”页面进行管理。 

### <a name="contributor-roles-vs-entity-roles"></a>参与者角色与实体角色

[实体角色](luis-concept-roles.md)适用于 LUIS 应用的数据模型。 协作者/参与者角色适用于创作访问级别。 

## <a name="moving-or-changing-ownership"></a>移动或更改所有权

应用通过其 Azure 资源进行定义，这取决于所有者的订阅。 

你可以移动自己的 LUIS 应用。 在 Azure 门户或 Azure CLI 中，请参阅以下文档资源：

* [在 LUIS 创作资源之间移动应用](https://{region}.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-move-app-to-another-luis-authoring-azure-resource)
* [将资源移到新的资源组或订阅](../../azure-resource-manager/resource-group-move-resources.md)
* [在同一订阅内移动资源或跨订阅移动资源](../../azure-resource-manager/move-limitations/app-service-move-limitations.md)

## <a name="access-for-private-and-public-apps"></a>专用和公共应用的访问权限

对于**专用**应用，所有者和参与者可以使用运行时访问权限。 对于**公共**应用，任何具有自己的 Azure [认知服务](../cognitive-services-apis-create-account.md)或 [LUIS](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) 运行时资源和公共应用 ID 的人员均可使用运行时访问权限。 

目前没有公共应用的目录。

### <a name="authoring-access"></a>创作访问
从 [LUIS](luis-reference-regions.md#luis-website) 门户或[创作 API](https://go.microsoft.com/fwlink/?linkid=2092087) 访问应用的权限由 Azure 创作资源控制。 

所有者和所有参与者具有创作应用所需的访问权限。 

|创作访问权限包括|注释|
|--|--|
|添加或删除终结点密钥||
|导出版本||
|导出终结点日志||
|导入版本||
|公开应用|如果应用公开，任何拥有创作或终结点密钥的人员都可以查询应用。|
|修改模型|
|发布|
|查看用于[主动学习](luis-how-to-review-endpoint-utterances.md)的终结点陈述|
|定型|

### <a name="prediction-endpoint-runtime-access"></a>预测终结点运行时访问权限

查询预测终结点所需的访问权限由“应用程序信息”  页上“管理”  部分的设置进行控制。 

![将应用设置为公共](./media/luis-concept-security/set-application-as-public.png)

|[专用终结点](#runtime-security-for-private-apps)|[公共终结点](#runtime-security-for-public-apps)|
|:--|:--|
|可供所有者和参与者使用|可供所有者、参与者以及知道应用 ID 的任何其他人使用|

可以通过在服务器到服务器环境中调用 LUIS 运行时密钥来控制谁可以查看该密钥。 如果在机器人上使用 LUIS，则机器人和 LUIS 之间的连接已经安全。 如果直接调用 LUIS 终结点，则应创建具有受控访问权限（如 [AAD](https://azure.microsoft.com/services/active-directory/)）的服务器端 API（如 Azure [函数](https://azure.microsoft.com/services/functions/)）。 如果调用并验证服务器端 API，则在确认授权后将调用传递到 LUIS。 尽管此策略不能防范中间人攻击，但它会针对用户模糊化处理你的密钥和终结点 URL，允许你跟踪访问，并允许你添加终结点响应日志记录（如 [Application Insights](https://azure.microsoft.com/services/application-insights/)）。

#### <a name="runtime-security-for-private-apps"></a>专用应用的运行时安全性

专用应用的运行时仅适用于：

|密钥和用户|说明|
|--|--|
|所有者的创作密钥| 最多 1000 个终结点命中数|
|协作者/参与者的创作密钥| 最多 1000 个终结点命中数|
|由作者或协作者/参与者分配给 LUIS 的任何密钥|基于密钥用法层|

#### <a name="runtime-security-for-public-apps"></a>公共应用的运行时安全性

应用配置为公共后，任何有效的 LUIS 创作密钥或 LUIS 终结点密钥都可以查询应用，只要该密钥未使用整个终结点配额  。

不是所有者或参与者的用户只有在获得应用 ID 时才能访问公共应用的运行时。 LUIS 不提供公共市场或其他搜索公共应用的方式  。  

公共应用在所有区域中发布，以便有基于区域的 LUIS 资源密钥的用户可以在与资源密钥关联的任何区域中访问该应用。

## <a name="securing-the-endpoint"></a>保护终结点安全 

可以通过在服务器到服务器环境中调用 LUIS 预测运行时终结点密钥来控制谁可以查看该密钥。 如果在机器人上使用 LUIS，则机器人和 LUIS 之间的连接已经安全。 如果直接调用 LUIS 终结点，则应创建具有受控访问权限（如 [AAD](https://azure.microsoft.com/services/active-directory/)）的服务器端 API（如 Azure [函数](https://azure.microsoft.com/services/functions/)）。 如果调用服务器端 API 并且身份验证和授权得到验证，则将调用传递到 LUIS。 尽管此策略不能防范中间人攻击，但它会针对用户模糊化处理你的终结点，允许你跟踪访问，并允许你添加终结点响应日志记录（如 [Application Insights](https://azure.microsoft.com/services/application-insights/)）。  

## <a name="next-steps"></a>后续步骤

* 了解[版本控制](luis-concept-version.md)概念。 
* 了解[如何创建密钥](luis-how-to-azure-subscription.md)。



