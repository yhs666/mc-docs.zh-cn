---
title: 动态字典 - 文本翻译 API
titlesuffix: Azure Cognitive Services
description: 如何使用文本翻译 API 的动态字典功能。
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
origin.date: 06/04/2019
ms.date: 07/11/2019
ms.author: v-junlch
ms.openlocfilehash: 8477006c3755f31a20185fcb4c9f504a62c5230d
ms.sourcegitcommit: 8f49da0084910bc97e4590fc1a8fe48dd4028e34
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844729"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>如何使用动态字典

若已知道要应用于某个单词或短语的翻译，可以在请求中将其作为标记提供。 动态词典仅适用于复合名词，例如专有名称和产品名称。

**语法：**

<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>

**示例：en-de：**

源输入：The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.

目标输出：Das Wort "wordomatic" ist ein Wörterbucheintrag.

无论使用还是不使用 HTML 模式，此功能都以相同的方式工作。

应尽量少使用此功能。

<!-- Update_Description: wording update -->