---
title: Azure Cosmos DB Gremlin 与 TinkerPop 功能的兼容性
description: 参考文档 Graph 引擎兼容性问题
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
origin.date: 09/10/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.openlocfilehash: 58350a9049ef195a69ddabdbef25bb5b737fb1fb
ms.sourcegitcommit: 73f07c008336204bd69b1e0ee188286d0962c1d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2019
ms.locfileid: "72970268"
---
# <a name="azure-cosmos-db-gremlin-compatibility"></a>Azure Cosmos DB Gremlin 兼容性
Azure Cosmos DB Graph 引擎紧密遵循 [Apache TinkerPop](https://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps) 遍历步骤规范，但是存在差异。

## <a name="behavior-differences"></a>行为差异

* Azure Cosmos DB Graph 引擎运行***广度优先***遍历，而 TinkerPop Gremlin 则深度优先。 这种行为在像 Cosmos DB 这样的水平可缩放系统中可实现更好的性能。 

## <a name="unsupported-features"></a>不支持的功能

* ***[Gremlin 字节码](http://tinkerpop.apache.org/docs/current/tutorials/gremlin-language-variants/)*** 是与编程语言无关的图遍历规范。 Cosmos DB Graph 尚不支持它。 请使用 ```GremlinClient.SubmitAsync()``` 并以文本字符串的形式传递遍历。

* ***```property(set, 'xyz', 1)```*** 目前不支持集基数。 请改用 ```property(list, 'xyz', 1)```。

* ***```match()```*** 允许使用声明性模式匹配来查询图。 此功能不可用。

* 不支持顶点或边上的***对象作为属性***。 属性只能是基元类型或数组。

* 不支持***按数组属性排序*** ```.order().by(<array property>)```。 只支持按基元类型排序。

* 不支持***非基元 JSON 类型***。 使用 ```string```、```number``` 或 ```true```/```false``` 类型。 不支持 ```null``` 值。 

* ***GraphSONv3*** 序列化程序当前不可用。

* 由于系统具有分布式特性，因此***事务***不受支持。  在 Gremlin 帐户上配置适当的一致性模型以“读取自己的写入”，并使用乐观并发解决冲突的写入。

## <a name="next-steps"></a>后续步骤
* 访问 [Cosmos DB 支持](https://support.azure.cn/support/contact/)页以共享反馈并帮助团队专注于对你重要的功能。

<!-- Update_Description: wording update -->
