---
title: 从 Azure 逻辑应用添加和调用 Azure 函数
description: 从逻辑应用添加和运行 Azure 函数
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: v-yiso
ms.topic: article
origin.date: 06/04/2019
ms.date: 08/26/2019
ms.reviewer: klam, LADocs
ms.openlocfilehash: 781daa89a7fb4224e7d1bb77734e65cc1f193ca9
ms.sourcegitcommit: d624f006b024131ced8569c62a94494931d66af7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69539011"
---
# <a name="call-azure-functions-from-azure-logic-apps"></a>从 Azure 逻辑应用调用 Azure 函数

若要在逻辑应用中运行执行特定作业的代码，可以使用 [Azure Functions](../azure-functions/functions-overview.md) 创建自己的函数。 该服务有助于创建 Node.js、C# 和 F# 函数，使你无需为运行代码而构建完整的应用或基础结构。 还能[从 Azure Functions 内部调用逻辑应用](#call-logic-app)。 Azure Functions 在云中提供无服务器计算，且对执行任务非常有用，如以下示例：

* 通过 Node.js 或 C# 函数扩展逻辑应用的行为。
* 在逻辑应用工作流中执行计算。
* 在逻辑应用中应用高级格式设置或计算字段。


> [!NOTE]
> 在逻辑应用和 Azure Functions 之间集成目前不适用于槽已启用的情况。

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果没有 Azure 订阅，请[注册一个 Azure 试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

* 一个 Azure 函数应用，它是 Azure Functions 和你的 Azure 函数的容器。 若没有函数应用，请先[创建函数应用](../azure-functions/functions-create-first-azure-function.md)。 然后才可以在逻辑应用外部（在 Azure 门户中）或[逻辑应用内部](#create-function-designer)（在逻辑应用设计器中）创建函数。

* 使用逻辑应用时，同样的要求适用于函数应用和函数，不管它们是现有的还是全新的：

  * 函数应用和逻辑应用必须使用同一 Azure 订阅。

  * 新的函数应用必须使用 .NET 或 JavaScript 作为运行时堆栈。 将新函数添加到现有的函数应用时，可以选择 C# 或 JavaScript。

  * 你的函数使用 **HTTP 触发器**模板。

    此 HTTP 触发器模板可从逻辑应用接受具有 `application/json` 类型的内容。 将 Azure 函数添加到逻辑应用中时，逻辑应用设计器在 Azure 订阅内显示基于此模板创建的自定义函数。

  * 函数不会使用自定义路由，除非定义了 OpenAPI 定义（以前称为 [Swagger 文件](https://swagger.io/)）。

  * 如果已经有函数的 OpenAPI 定义，逻辑应用设计器会提供更丰富的函数参数使用体验。 在逻辑应用查找并访问具有 OpenAPI 定义的函数之前，请先[按以下步骤设置函数应用](#function-swagger)。

* 要在其中添加函数的逻辑应用（加入[触发器](../logic-apps/logic-apps-overview.md#logic-app-concepts)是逻辑应用中的第一步）

  在添加运行函数的操作之前，必须使用触发器启动逻辑应用。 如果不熟悉逻辑应用，请查看[什么是 Azure 逻辑应用](../logic-apps/logic-apps-overview.md)和[快速入门：创建第一个逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。

<a name="function-swagger"></a>

## <a name="find-functions-that-have-openapi-descriptions"></a>查找有 OpenAPI 说明的函数

在逻辑应用设计器中使用函数参数时，若要得到更丰富的体验，请为函数生成 OpenAPI 定义（以前称为 [Swagger 文件](https://swagger.io/)）。 若要设置函数应用以使其可查找和使用具备 Swagger 描述的函数，请按照以下步骤操作：

1. 确保函数应用正在运行。

1. 按照以下步骤，在函数应用中，设置[跨源资源共享 (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) 以便允许所有来源：

   1. 从“函数应用”列表中选择自己的函数应用。  在右侧窗格中，选择“平台功能”   >   “CORS”。

      ![选择函数应用 >“平台功能”>“CORS”](./media/logic-apps-azure-functions/function-platform-features-cors.png)

   1. 在“CORS”下，添加星号 ( **`*`** ) 通配符，但删除列表中的所有其他来源并选择“保存”   。

      ![将 CORS 设为通配符“*”](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

## <a name="access-property-values-inside-http-requests"></a>访问 HTTP 请求中的属性值

Webhook 函数能接受 HTTP 请求作为输入，并将这些请求传递至其他函数。 例如，尽管逻辑应用具有[转换 DateTime 值的函数](../logic-apps/workflow-definition-language-functions-reference.md)，但此基本示例 JavaScript 函数演示了如何访问传递给函数的请求对象中的属性，并对该属性值执行操作。 为访问对象内的属性，此示例使用[点 (.) 运算符](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Property_accessors)：

```javascript
function convertToDateString(request, response){
   var data = request.body;
   response = {
      body: data.date.ToDateString();
   }
}
```

下面是此函数内部发生的情况：

1. 该函数创建 `data` 变量并将 `request` 对象内的 `body` 对象分配给该变量。 该函数使用点 (.) 运算符引用 `request` 对象内的 `body` 对象：

   ```javascript
   var data = request.body;
   ```

1. 该函数现在可以通过 `data` 变量访问 `date` 属性，并通过调用 `ToDateString()` 函数将该属性值从 DateTime 类型转换为 DateString 类型。 该函数还通过函数响应中的 `body` 属性返回结果：

   ```javascript
   body: data.date.ToDateString();
   ```

现已创建自己的 Azure 函数，请按步骤[将函数添加到逻辑应用](#add-function-logic-app)。

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>在逻辑应用内部创建函数

若要在逻辑应用编辑器中从逻辑应用内部创建 Azure 函数，必须先具备一个 Azure 函数应用，这是函数的容器。 若没有函数应用，请先创建一个。 请参阅[在 Azure 门户中创建第一个函数](../azure-functions/functions-create-first-azure-function.md)。

1. 在 [Azure 门户](https://portal.azure.cn)的逻辑应用设计器中打开逻辑应用。

1. 按照适用于自身方案的步骤，创建并添加函数：

   * 如果处于逻辑应用工作流最后一步，请选择“新步骤”  。

   * 如果介于逻辑应用工作流中现有步骤之间，请将鼠标移至箭头上，选择加号 (+)，然后选择“添加操作”  。

1. 在搜索框中输入“Azure Functions”作为筛选器。 在操作列表中选择此操作：**选择 Azure 函数 - Azure Functions**

   ![找到“Azure Functions”](./media/logic-apps-azure-functions/find-azure-functions-action.png)

1. 从函数应用列表中选择自己的函数应用。 在操作列表打开后，选择以下操作：**Azure Functions - 创建新的函数**

   ![选择函数应用](./media/logic-apps-azure-functions/select-function-app-create-function.png)

1. 在函数定义编辑器中定义函数：

   1. 在“函数名称”框中提供函数的名称  。

   1. 在“代码”框中，将代码添加至函数模板，包括函数运行完成后想返回至逻辑应用的响应和有效负载  。

      ![定义函数](./media/logic-apps-azure-functions/function-definition.png)

      模板代码中的 `context` 对象表示逻辑应用通过后续步骤中“请求正文”字段发送的消息   。 要从函数内访问 `context` 对象的属性，请使用如下语法：

      `context.body.<property-name>`

      例如，要引用 `context` 对象内的 `content` 属性，请使用如下语法： 

      `context.body.content`

      此模板代码还包含一个 `input` 变量，此变量存储来自 `data` 参数的值，因此函数可对该值执行操作。 在 JavaScript 函数内，`data` 变量也是 `context.body` 的一种快捷方式。

      > [!NOTE]
      > 此处的 `body` 属性适用于 `context` 对象，但与来自操作输出的 Body 令牌不同，你可能也会希望将后者传递到函数  。

   1. 完成后，选择“创建”  。

1. 在“请求正文”框中，提供函数的输入，其格式必须为 JavaScript 对象表示法 (JSON) 对象  。

   此输入是逻辑应用发送到函数的上下文对象或消息  。 点击“请求正文”字段时，界面会显示动态内容列表，以便你可以为先前步骤中的输出选择令牌  。 本示例指定上下文有效负载包含一个名为 `content` 的属性，该属性具有来自电子邮件触发器的 From 令牌的值  ：

   ![“请求正文”示例 - 上下文对象有效负载](./media/logic-apps-azure-functions/function-request-body-example.png)

   此处的上下文对象没有强制转换为字符串，因此对象的内容被直接添加到 JSON 有效负载中。 但是，如果该上下文对象不是传递字符串、JSON 对象或 JSON 数组的 JSON 令牌，则会出现错误。 因此，假如本示例使用 Received Time 令牌，则可通过添加双引号将此上下文对象强制转换为字符串  ：  

   ![将对象强制转换为字符串](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

1. 若要指定其他详细信息，例如要使用的方法、请求标头或查询参数，请打开“添加新参数”列表，然后选择所需选项  。

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>将现有函数添加至逻辑应用

若要从逻辑应用调用现有 Azure 函数，可以添加 Azure 函数，具体方法与在逻辑应用设计器中执行的任何其他操作一样。

1. 在 [Azure 门户](https://portal.azure.cn)的逻辑应用设计器中打开逻辑应用。

1. 在要添加函数的步骤下，选择“新建步骤”，然后选择“添加操作”。  

1. 在搜索框中输入“Azure Functions”作为筛选器。 在操作列表中选择此操作：**选择 Azure 函数 - Azure Functions**

   ![找到“Azure Functions”](./media/logic-apps-azure-functions/find-azure-functions-action.png)

1. 从函数应用列表中选择自己的函数应用。 在显示的函数列表中选择函数。

   ![选择函数应用和 Azure 函数](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   对于具备 API 定义（Swagger 描述）的函数以及那些[设置为可供逻辑应用查找和访问](#function-swagger)的函数，可以选择“Swagger 操作”  :

   ![选择函数应用“Swagger 操作”以及 Azure 函数](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

1. 在“请求正文”框中，提供函数的输入，其格式必须为 JavaScript 对象表示法 (JSON) 对象  。

   此输入是逻辑应用发送到函数的上下文对象或消息  。 点击“请求正文”字段时，界面会显示动态内容列表，以便你可以为先前步骤中的输出选择令牌  。 本示例指定上下文有效负载包含一个名为 `content` 的属性，该属性具有来自电子邮件触发器的 From 令牌的值  ：

   ![“请求正文”示例 - 上下文对象有效负载](./media/logic-apps-azure-functions/function-request-body-example.png)

   此处的上下文对象没有强制转换为字符串，因此对象的内容被直接添加到 JSON 有效负载中。 但是，如果该上下文对象不是传递字符串、JSON 对象或 JSON 数组的 JSON 令牌，则会出现错误。 因此，假如本示例使用 Received Time 令牌，则可通过添加双引号将此上下文对象强制转换为字符串  ：

   ![将对象强制转换为字符串](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

1. 若要指定其他详细信息，例如要使用的方法、请求标头或查询参数，请打开“添加新参数”列表，然后选择所需选项  。

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-azure-functions"></a>从 Azure 函数调用逻辑应用

若要从 Azure 函数内部触发逻辑应用，则必须使用提供可调用终结点的触发器启动该逻辑应用。 例如，可使用 HTTP 触发器、请求触发器、Azure 队列触发器或事件网格触发器启动逻辑应用     。 在函数内，向触发器的 URL 发送一个 HTTP POST 请求，并加入需要逻辑应用处理的有效负载。 有关详细信息，请参阅[调用、触发器或嵌套逻辑应用](../logic-apps/logic-apps-http-endpoint.md)。

## <a name="next-steps"></a>后续步骤

* 了解[逻辑应用连接器](../connectors/apis-list.md)
