---
title: Azure 流分析中的输出错误策略
description: 了解 Azure 流分析中提供的输出错误处理策略。
services: stream-analytics
author: lingliw
ms.author: v-lingwu
manager: digimobile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
origin.date: 08/09/2019
ms.date: 06/21/2019
ms.openlocfilehash: d2f3ce5409f62557cb4f94859421799d12072a35
ms.sourcegitcommit: 3702f1f85e102c56f43d80049205b2943895c8ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2019
ms.locfileid: "68970458"
---
# <a name="azure-stream-analytics-output-error-policy"></a>Azure 流分析的输出错误策略
本文介绍可在 Azure 流分析中配置的输出数据错误处理策略。

输出数据错误处理策略仅适用于当流分析作业生成的输出事件不符合目标接收器的架构时发生的数据转换错误。 可以选择“重试”或“丢弃”来配置此策略。   在 Azure 门户上的流分析作业中的“配置”下选择“错误策略”。  

![Azure 流分析的输出错误策略位置](./media/stream-analytics-output-error-policy/stream-analytics-error-policy-locate.png)


## <a name="retry"></a>重试
发生错误时，Azure 流分析会无限期重试写入事件，直到写入成功为止。 重试不会超时。 最终，重试的事件会阻止所有后续事件的处理。 此选项是默认的输出错误处理策略。

## <a name="drop"></a>丢弃
Azure 流分析会丢弃任何导致数据转换错误的输出事件。 无法恢复丢弃的事件以便稍后重新处理。


不管采用哪种输出错误处理策略配置，都会重试所有暂时性错误（例如网络错误）。


## <a name="next-steps"></a>后续步骤
[Azure 流分析故障排除指南](stream-analytics-troubleshooting-guide.md)
