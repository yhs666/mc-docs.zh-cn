---
title: 动态字典 - 文本翻译 API
titlesuffix: Azure Cognitive Services
description: 如何使用文本翻译 API 的动态字典功能。
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
origin.date: 02/21/2019
ms.date: 06/11/2019
ms.author: v-junlch
ms.openlocfilehash: 8a093c0af666c5c231618812d29102aa2ca8772d
ms.sourcegitcommit: 259c97c9322da7add9de9f955eac275d743c9424
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2019
ms.locfileid: "66830114"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-translator-text-api"></a>如何使用文本翻译 API 的动态字典功能

若已知道要应用于某个单词或短语的翻译，可以在请求中将其作为标记提供。 动态词典仅适用于复合名词，例如专有名称和产品名称。

**语法：**

<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>

**示例：en-de：**

源输入：The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.

目标输出：Das Wort "wordomatic" ist ein Wörterbucheintrag.

无论使用还是不使用 HTML 模式，此功能都以相同的方式工作。

应尽量少使用此功能。

<!-- Update_Description: update metedata properties -->