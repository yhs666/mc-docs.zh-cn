---
title: "计算机视觉 API 常见问题解答 | Microsoft Docs"
description: "获取有关 Microsoft 认知服务中计算机视觉 API 的常见问题的解答。"
services: cognitive-services
author: alexchen2016
manager: digimobile
ms.service: cognitive-services
ms.technology: computer-vision
ms.topic: article
origin.date: 01/26/2017
ms.date: 10/13/2017
ms.author: v-junlch
ms.openlocfilehash: 4149261122c2b8cc8e799a5b018b6b99eff17d74
ms.sourcegitcommit: 9b2b3a5aede3a66aaa5453e027f1e7a56a022d49
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2017
---
# <a name="computer-vision-api-frequently-asked-questions"></a>计算机视觉 API 常见问题解答
### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-computer-vision-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>如果在本“常见问题解答”中找不到问题的答案，请尝试在 [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) 中向计算机视觉 API 社区提问，或联系 [UserVoice 上的帮助和支持人员](https://cognitive.uservoice.com/)

-----

**问**：*是否可将计算机视觉 API 训练为使用自定义标记？例如，我想要馈送猫的种类图片以“训练”AI，然后在 AI 请求中接收种类值。*

**答**：目前未推出此功能。 但是，我们的工程师正在努力为计算机视觉开发此功能。

-----

**问**：*是否可以在不连接 Internet 的情况下在本地使用计算机视觉？*

**答**：我们目前不提供本地解决方案。

-----

**问**：*计算机视觉支持哪些语言？*

**答**：支持的语言包括：

| | | 支持的语言 | | |
|---------------- |------------------ |------------------ |--------------------------- |--------------------
| 丹麦语 (da-DK)  | 荷兰语 (nl-NL)     | 英语           | 芬兰语 (fi-FI)            |法语 (fr-FR)
| 德语 (de-DE)  | 希腊语 (el-GR)     | 匈牙利语 (hu-HU) | 意大利语 (it-IT)            | 日语 (ja-JP)
| 韩语 (ko-KR)  | 挪威语 (nb-NO) | 波兰语 (pl-PL)    | 葡萄牙语 (pt-BR) (pt-PT) | 俄语 (ru-RU)
| 西班牙语 (es-ES)   | 瑞典语 (sv-SV)     | 土耳其语 (tr-TU)   |                            |

-----

**问**：*是否可以使用计算机视觉来读取牌照？*

**答**：视觉 API 通过 OCR 提供良好的文本检测，但目前未针对牌照进行优化。 我们正在不断尝试改进服务，并在功能请求列表中添加了用于自动识别牌照的 OCR。

-----

**问：***手写识别支持哪些语言？*

**答**：目前仅支持英语。

-----

**问：***手写识别支持哪些类型的书写表面？*

**答**：该技术适用于不同种类的表面，包括白板、白纸和黄色便笺。

-----

**问**：*手写识别操作需要多长时间？*

**答**：所需的时间取决于文本长度。 对于较长的文本，可能要花费几秒钟时间。 因此，在完成“识别手写文本”操作后，可能需要等待一段时间，然后才能使用“获取手写文本操作结果”操作检索结果。

-----

**问**：*手写识别技术如何处理在一行中间使用脱字号插入的文本？*

**答**：手写识别操作以独行的形式返回此类文本。

-----

**问**：*手写识别技术如何处理划掉的单词或行？*

**答**：如果使用多条删除线划掉了单词，以至无法识别这些单词，则手写识别操作不会拾取这些单词。 但是，如果单词是使用单条删除线划掉的，则这种划线被视为干扰元素，手写识别操作仍会拾取这些单词。

-----

**问**：*手写识别技术支持哪些文本方向？*

**答**：手写识别操作可以拾取朝向最大约为 30 度到 40 度角排列的文本。

-----

