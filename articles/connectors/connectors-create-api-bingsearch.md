---
title: 连接到必应搜索 - Azure 逻辑应用 | Microsoft Docs
description: 使用必应搜索 REST API 和 Azure 逻辑应用查找新闻
author: ecfan
manager: jeconnoc
ms.author: v-yiso
origin.date: 05/21/2018
ms.date: 07/02/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: dc0fd2c033007ffbc86f9b833450e1f1fe94d99a
ms.sourcegitcommit: 092d9ef3f2509ca2ebbd594e1da4048066af0ee3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "36315586"
---
# <a name="find-news-with-bing-search-and-azure-logic-apps"></a>使用必应搜索和 Azure 逻辑应用查找新闻 

本文介绍如何使用必应搜索连接器在逻辑应用中通过必应搜索查找新闻、视频和其他项目。 可以通过这种方式创建逻辑应用，以便自动完成进行搜索结果处理所需的任务和工作流，并使这些项目可供其他操作使用。 

例如，可以根据搜索条件查找新闻项目，并通过推特在自己的推特源中将这些项目发布为推文。

如果没有 Azure 订阅，请<a href="https://www.azure.cn/pricing/1rmb-trial/" target="_blank">注册一个 Azure 试用帐户</a>。 如果不熟悉逻辑应用，请查看[什么是 Azure 逻辑应用](../logic-apps/logic-apps-overview.md)和[快速入门：创建第一个逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。
如需特定于连接器的技术信息，请参阅<a href="https://docs.microsoft.com/connectors/bingsearch/" target="blank">必应搜索连接器参考</a>。

## <a name="prerequisites"></a>先决条件

* 一个[认知服务帐户](../cognitive-services/cognitive-services-apis-create-account.md)

* 一个[必应搜索 API 密钥](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api)，用于从逻辑应用访问必应搜索 API 

* 要在其中访问事件中心的逻辑应用。 若要通过必应搜索触发器启动逻辑应用，需要一个[空白逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。 

<a name="add-trigger"></a>

## <a name="add-a-bing-search-trigger"></a>添加必应搜索触发器

在 Azure 逻辑应用中，每个逻辑应用都必须从[触发器](../logic-apps/logic-apps-overview.md#logic-app-concepts)开始，该触发器在发生特定事件或特定条件得到满足的情况下触发。 每当触发器触发时，逻辑应用引擎就会创建一个逻辑应用实例并开始运行应用的工作流。

1. 在 Azure 门户或 Visual Studio 中创建一个空白的逻辑应用，以便打开逻辑应用设计器。 此示例使用 Azure 门户。

2. 在搜索框中，输入“必应搜索”作为筛选器。 在触发器列表中，选择所需的触发器。 

   本示例使用“必应搜索 - 针对新的新闻文章”触发器

   ![查找必应搜索触发器](./media/connectors-create-api-bing-search/add-trigger.png)

3. 如果系统提示输入连接详细信息，请[立即创建必应搜索连接](#create-connection)。 或者，如果连接已存在，请提供触发器所需的信息。

   就本示例来说，请提供相关条件，以便从必应搜索返回与之匹配的新闻文章。

   | 属性 | 必须 | 值 | 说明 | 
   |----------|----------|-------|-------------| 
   | 搜索查询 | 是 | <*search-words*> | 输入想要使用的搜索关键字。 |
   | 市场 | 是 | <*locale*> | 搜索区域设置。 默认为“en-US”，但可以选择其他值。 | 
   | 安全搜索 | 是 | <*search-level*> | 用于排除成人内容的筛选器级别。 默认为“中等”，但可以选择其他级别。 | 
   | 计数 | 否 | <*results-count*> | 返回指定数量的结果。 默认为 20，但可以指定其他值。 所返回结果的实际数量可能少于指定的数量。 | 
   | Offset | 否 | <*skip-value*> | 在返回结果前需跳过的结果数 | 
   ||||| 

   例如：

   ![设置触发器](./media/connectors-create-api-bing-search/bing-search-trigger.png)

4. 选择你希望触发器以多大时间间隔和频率来检查结果。

5. 完成后，请在设计器工具栏上选择“保存”。 

6. 现在请继续向逻辑应用添加一个或多个操作，以便完成需对触发器结果执行的任务。

<a name="add-action"></a>

## <a name="add-a-bing-search-action"></a>添加必应搜索操作

在 Azure 逻辑应用中，[操作](../logic-apps/logic-apps-overview.md#logic-app-concepts)是指工作流中紧跟在某个触发器或另一操作后面执行的一个步骤。 就此示例来说，逻辑应用一开始使用必应搜索触发器来返回与指定条件匹配的新闻文章。 

1. 在 Azure 门户或 Visual Studio 的逻辑应用设计器中打开逻辑应用。 此示例使用 Azure 门户。

2. 在该触发器或操作下，选择“新建步骤” > “添加操作”。

   本示例使用“必应搜索 - 针对新的新闻文章”触发器

   ![添加操作](./media/connectors-create-api-bing-search/add-action.png)

   若要在现有步骤之间添加操作，请将鼠标移到连接箭头上方。 
   选择出现的加号 (**+**)，然后选择“添加操作”。

3. 在搜索框中，输入“必应搜索”作为筛选器。
从操作列表中选择所需的操作。

   本示例使用“必应搜索 - 按查询列出新闻”操作

   ![查找必应搜索操作](./media/connectors-create-api-bing-search/bing-search-select-action.png)

4. 如果系统提示输入连接详细信息，请[立即创建必应搜索连接](#create-connection)。 或者，如果连接已存在，请提供操作所需的信息。 

   就本示例来说，请提供相关条件，以便返回一部分触发器结果。

   | 属性 | 必须 | 值 | 说明 | 
   |----------|----------|-------|-------------| 
   | 搜索查询 | 是 | <*search-expression*> | 输入用于查询触发器结果的表达式。 可以从动态内容列表的字段中进行选择，或使用表达式生成器创建表达式。 |
   | 市场 | 是 | <*locale*> | 搜索区域设置。 默认为“en-US”，但可以选择其他值。 | 
   | 安全搜索 | 是 | <*search-level*> | 用于排除成人内容的筛选器级别。 默认为“中等”，但可以选择其他级别。 | 
   | 计数 | 否 | <*results-count*> | 返回指定数量的结果。 默认为 20，但可以指定其他值。 所返回结果的实际数量可能少于指定的数量。 | 
   | Offset | 否 | <*skip-value*> | 在返回结果前需跳过的结果数 | 
   ||||| 

   例如，假设需要类别名称包含单词“tech”的结果。 
   
   1. 在“搜索查询”框中单击，以显示动态内容列表。 
   从该列表中选择“表达式”，以显示表达式生成器。 

      ![必应搜索触发器](./media/connectors-create-api-bing-search/bing-search-action.png)

      现在即可开始创建表达式。

   2. 从函数列表中选择“contains()”函数，该函数将显示在表达式框中。 单击“动态内容”，以便重新显示字段列表，但请确保将光标停留在圆括号内。

      ![选择函数](./media/connectors-create-api-bing-search/expression-select-function.png)

   3. 从字段列表中选择将转换为参数的“类别”。 
   在第一个参数后添加一个逗号，并在该逗号后加上此单词：`'tech'` 

      ![选择字段](./media/connectors-create-api-bing-search/expression-select-field.png)
   
   4. 完成后，选择“确定”。

      该表达式现在以此格式显示在“搜索查询”框中：

      ![已完成的表达式](./media/connectors-create-api-bing-search/resolved-expression.png)

      在代码视图中，此表达式以此格式显示：

      `"@{contains(triggerBody()?['category'],'tech')}"`

5. 完成后，请在设计器工具栏上选择“保存”。

<a name="create-connection"></a>

## <a name="connect-to-bing-search"></a>连接到必应搜索

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. 系统提示输入连接信息时，请提供以下详细信息：

   | 属性 | 必须 | 值 | 说明 | 
   |----------|----------|-------|-------------| 
   | 连接名称 | 是 | <*connection-name*> | 将要为连接创建的名称 |
   | API 版本 | 是 | <*API-version*> | 默认情况下，必应搜索 API 版本设置为当前版本。 可以根据需要选择早期版本。 | 
   | API 密钥 | 是 | <*API-key*> | 之前获取的必应搜索 API 密钥。 如果没有密钥，请立刻获取 [API 密钥](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api)。 |  
   |||||  

   例如：

   ![创建连接](./media/connectors-create-api-bing-search/bing-search-create-connection.png)

2. 完成后，选择“创建”。

## <a name="connector-reference"></a>连接器参考

如需技术详细信息（例如触发器、操作和限制，如连接器的 Swagger 文件所述），请查看[连接器的参考页](/connectors/bingsearch/)。 

## <a name="get-support"></a>获取支持

* 有关问题，请访问 [Azure 逻辑应用论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps)。
* 若要提交功能建议或对功能建议进行投票，请访问[逻辑应用用户反馈网站](http://aka.ms/logicapps-wish)。

## <a name="next-steps"></a>后续步骤

* 了解其他[逻辑应用连接器](../connectors/apis-list.md)