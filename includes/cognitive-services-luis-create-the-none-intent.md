---
title: include 文件
description: include 文件
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: e3e52e182b6beb61ae24c3fd0e6174a28cb704b9
ms.sourcegitcommit: bf4c3c25756ae4bf67efbccca3ec9712b346f871
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2019
ms.locfileid: "65555754"
---
客户端应用程序需要知道某个话语是否对应用程序没有意义或不合适。 在创建过程中，“None”意向将添加到每个应用程序，以确定客户端应用程序是否无法回答某个话语。

如果 LUIS 对于某个话语返回“None”意向，客户端应用程序可以询问用户是否要结束聊天或提供更多指示以继续聊天。 

> [!CAUTION] 
> 不要将“None”意向留空。 

1. 在左侧面板中选择“意向”。

2. 选择“None”意向。 添加用户可能会输入但与人力资源应用无关的 3 个话语：

    | 示例陈述|
    |--|
    |Barking dogs are annoying|
    |Order a pizza for me|
    |Penguins in the ocean|
