---
title: 短语列表 - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何使用 `PhraseListGrammar` 对象为语音服务提供短语列表，以便改进语音转文本的识别结果。
services: cognitive-services
author: rhurey
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 07/05/2019
ms.date: 11/25/2019
ms.author: v-tawe
ms.openlocfilehash: 5d7e9431f010dd54c4968b272ef2c84c14dbd929
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74390008"
---
# <a name="phrase-lists-for-speech-to-text"></a>语音转文本的短语列表

可以通过提供带短语列表的语音服务，改进语音识别的准确度。 短语列表用于确定音频数据中的已知短语，例如某个人的姓名或特定位置。

例如，如果命令为“Move to”，可能的目标为可以说出来的“Ward”，则可添加条目“Move to Ward”。 添加短语以后，在识别该音频时，将其识别成“Move to Ward”而不是“Move toward”的可能性就会增加。

可以向短语列表添加单个单词或完整短语。 在识别过程中，如果音频中包含某个完全匹配项，则会使用短语列表中的条目。 以上面的示例为基础，假设短语列表包含“Move to Ward”，而音频捕获的声音既类似于“Move to Ward”，又类似于“Move to Ward”，则识别结果更可能为“Move to Ward slowly”。

>[!Note]
> 目前，短语列表仅支持英语版语音转文本。

## <a name="how-to-use-phrase-lists"></a>如何使用短语列表

以下示例演示了如何使用 `PhraseListGrammar` 对象构建短语列表。

```C++
auto phraselist = PhraseListGrammar::FromRecognizer(recognizer);
phraselist->AddPhrase("Move to Ward");
phraselist->AddPhrase("Move to Bill");
phraselist->AddPhrase("Move to Ted");
```

```cs
PhraseListGrammar phraseList = PhraseListGrammar.FromRecognizer(recognizer);
phraseList.AddPhrase("Move to Ward");
phraseList.AddPhrase("Move to Bill");
phraseList.AddPhrase("Move to Ted");
```

```Python
phrase_list_grammar = speechsdk.PhraseListGrammar.from_recognizer(reco)
phrase_list_grammar.addPhrase("Move to Ward")
phrase_list_grammar.addPhrase("Move to Bill")
phrase_list_grammar.addPhrase("Move to Ted")
```

```JavaScript
var phraseListGrammar = SpeechSDK.PhraseListGrammar.fromRecognizer(reco);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

```Java
PhraseListGrammar phraseListGrammar = PhraseListGrammar.fromRecognizer(recognizer);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

>[!Note]
> 语音服务用于匹配语音的短语列表的最大数目为 1024 个短语。

也可通过调用 clear() 来清理与 `PhraseListGrammar` 关联的短语。

```C++
phraselist->Clear();
```

```cs
phraseList.Clear();
```

```Python
phrase_list_grammar.clear()
```

```JavaScript
phraseListGrammar.clear();
```

```Java
phraseListGrammar.clear();
```

> [!NOTE]
> 对 `PhraseListGrammar` 对象的更改会在下次识别时生效，或者会在重新连接到语音服务后生效。

## <a name="next-steps"></a>后续步骤

* [语音 SDK 参考文档](speech-sdk.md)
