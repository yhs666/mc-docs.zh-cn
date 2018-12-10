---
title: 动态字典 - 文本翻译 API
titlesuffix: Azure Cognitive Services
description: 如何使用文本翻译 API 的动态字典功能。
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
origin.date: 12/14/2017
ms.date: 11/26/2018
ms.author: v-junlch
ms.openlocfilehash: 4206475b10a4496b074502a9176fa59049b2d3c6
ms.sourcegitcommit: bfd0b25b0c51050e51531fedb4fca8c023b1bf5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52673484"
---
# <a name="how-to-use-the-dynamic-dictionary-feature-of-the-translator-text-api"></a>如何使用文本翻译 API 的动态字典功能

若已知道要应用于某个单词或短语的翻译，可以在请求中将其作为标记提供。 动态词典仅适用于复合名词，例如专有名称和产品名称。

**语法：**

<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>

**示例：en-de：**

源输入：The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.

目标输出：Das Wort "wordomatic" ist ein Wörterbucheintrag.

无论使用还是不使用 HTML 模式，此功能都以相同的方式工作。

应尽量少使用此功能。 对翻译进行自定义时，一个合适且要好得多的方法是使用自定义翻译工具。 自定义翻译工具能够充分利用上下文和统计概率。 如果必须或能够创建在上下文中显示工作或短语的训练数据，则会得到更好的结果。 可以在 [http://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator) 中找到有关自定义翻译的详细信息。

