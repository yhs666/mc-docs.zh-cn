---
title: Azure 流分析中的输出错误策略
description: 了解 Azure 流分析中提供的输出错误处理策略。
services: stream-analytics
author: rockboyfor
ms.author: v-yeche
manager: digimobile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
origin.date: 07/27/2018
ms.date: 09/17/2018
ms.openlocfilehash: 089e059e07ba70d082dadda1eadc2ad04412020a
ms.sourcegitcommit: 2700f127c3a8740a83fb70739c09bd266f0cc455
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/14/2018
ms.locfileid: "45586653"
---
# <a name="output-error-policy"></a>输出错误策略
本文介绍可在 Azure 流分析中配置的输出数据错误处理策略。

输出数据错误处理策略仅适用于当流分析作业生成的输出事件不符合目标接收器的架构时发生的数据转换错误。 可以选择“重试”或“丢弃”来配置此策略。 在 Azure 门户上的流分析作业中的“配置”下选择“错误策略”。

![错误策略 - 位置](./media/stream-analytics-error-policy/stream-analytics-error-policy-locate.PNG)

## <a name="retry"></a>重试
发生错误时，Azure 流分析会无限期重试写入事件，直到写入成功为止。 重试不会超时。 最终，重试的事件会阻止所有后续事件的处理。 此选项是默认的输出错误处理策略。

## <a name="drop"></a>丢弃
Azure 流分析会丢弃任何导致数据转换错误的输出事件。 无法恢复丢弃的事件以便稍后重新处理。

不管采用哪种输出错误处理策略配置，都会重试所有暂时性错误（例如网络错误）。

## <a name="next-steps"></a>后续步骤
[Azure 流分析故障排除指南](stream-analytics-troubleshooting-guide.md)

<!-- Update_Description: new articles on stream analytics output error policy -->
<!--ms.date: 09/17/2018-->