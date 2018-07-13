---
title: 使用 Azure 流分析作业避免服务中断
description: 本文介绍有关生成流分析作业升级复原力的指南。
services: stream-analytics
author: rockboyfor
manager: digimobile
ms.author: v-yeche
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
origin.date: 03/28/2017
ms.date: 06/18/2018
ms.openlocfilehash: d82abeaaba048717c5e4b4cda2d93b74b50b8cef
ms.sourcegitcommit: 18810626635f601f20550a0e3e494aa44a547f0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2018
ms.locfileid: "37405279"
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>在服务更新期间保证流分析作业可靠性

作为完全托管服务的一部分的是快速引入新服务功能和改进的能力。 因此，流分析可以每周（或更频繁地）进行服务更新部署。 无论进行多少次测试，由于引入了 bug，仍存在现有正在运行的作业可能会中断的风险。 对于运行关键流式处理作业的客户来说，需要避免这些风险。 客户可以用来降低此风险的机制是 Azure 的**配对区域**模型。 
<!-- Not Available on [paired region](/best-practices-availability-paired-regions)-->

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Azure 配对区域如何解决此问题？

流分析可以保证在单独的批处理中更新配对区域中的作业。 因此，在更新之间具有足够的时间间隔来识别潜在的重大 bug 并修复它们。

流分析的更新部署不会同时在一组配对区域中进行。 **同一组中**多个区域中的部署可能会**同时**进行。
<!-- Notice: Remove the India exception -->
<!-- Not Available on [availability and paired regions](/best-practices-availability-paired-regions--> 建议客户将相同的作业部署到这两个配对区域。 除了流分析的内部监视功能外，还建议客户监视作业，就好像**这两个作业**是生产作业一样。 如果中断确定为流分析服务更新的结果，请相应地升级并将任何下游使用者故障转移到正常运行的作业输出。 升级到支持部门可防止配对区域受新部署的影响并维护配对作业的完整性。

<!-- Update_Description: new articles on stream analytics job reliability -->
<!--ms.date: 06/18/2018-->