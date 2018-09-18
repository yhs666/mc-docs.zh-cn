---
title: include 文件
description: include 文件
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
origin.date: 07/17/2018
ms.date: 08/31/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: dde156c8ee8ee347dab457f4ce7359624cf59717
ms.sourcegitcommit: d834bdabbec1713d0378dd8bcffa5bf27d062e0b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45522113"
---
1. 选择 Azure 门户左上角的“新建资源”按钮，然后选择“计算” > “Function App”。 

    ![在 Azure 门户中创建函数应用](./media/functions-create-function-app-portal/function-app-create-flow.png)

2. 使用图像下面的表格中指定的函数应用设置。

    ![定义新的函数应用设置](./media/functions-create-function-app-portal/function-app-create-flow2.png)

    | 设置      | 建议的值  | 说明                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **应用名称** | 全局唯一名称 | 用于标识新 Function App 的名称。 有效的字符是 `a-z`、`0-9` 和 `-`。  | 
    | **订阅** | 订阅 | 要在其下创建此新函数应用的订阅。 | 
    | [资源组](../articles/azure-resource-manager/resource-group-overview.md) |  myResourceGroup | 要在其中创建 Function App 的新资源组的名称。 | 
    | [存储帐户](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) |  全局唯一名称 |  Function App 使用的新存储帐户的名称。 存储帐户名称必须为 3 到 24 个字符，并且只能包含数字和小写字母。 也可使用现有帐户，但该帐户必须符合[存储帐户要求](../articles/azure-functions/functions-scale.md#storage-account-requirements)。 |
    | **应用服务计划/位置** |  | 按应用服务计划运行时，必须管理[函数应用的缩放](../articles/azure-functions/functions-scale.md)。  |

3. 选择“创建”以预配和部署函数应用。

4. 选择门户右上角的“通知”图标，留意是否显示“部署成功”消息。 

    ![定义新的函数应用设置](./media/functions-create-function-app-portal/function-app-create-notification.png)

5. 选择“转到资源”，查看新的函数应用。

> [!TIP]
> 如果在门户中找不到函数应用，请尝试[将 Function App 添加到 Azure 门户中的收藏夹](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#favorite)。   

<!-- ms.date: 08/31/2018 -->