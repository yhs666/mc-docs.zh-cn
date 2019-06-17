---
title: 了解 Azure Monitor 警报的自愿性迁移工具的工作原理
description: 了解警报迁移工具的工作原理和如何排查问题。
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 4c405df0b34317bae4af7209532979385fba27ce
ms.sourcegitcommit: f818003595bd7a6aa66b0d3e1e0e92e79b059868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2019
ms.locfileid: "66732344"
---
# <a name="understand-how-the-migration-tool-works"></a>了解迁移工具的工作原理

根据[之前的通告](monitoring-classic-retirement.md)，Azure Monitor 中的经典警报即将在 2019 年 7 月停用。 Azure 门户中为使用经典警报规则并想要自行触发迁移的客户提供了一个迁移工具。

本文介绍自愿性迁移工具的工作原理。 此外还介绍了一些常见问题的补救措施。

## <a name="which-classic-alert-rules-can-be-migrated"></a>可以迁移哪些经典警报规则？

该工具几乎可以迁移所有的经典警报规则，不过也存在一些例外情况。 使用该工具（或者在 2019 年 7 月自动迁移期间）无法迁移以下警报规则：

- 针对虚拟机来宾指标的经典警报规则（Windows 和 Linux）。 请参阅本文稍后所述的[有关在新指标警报中重新创建此类警报规则的指导](#guest-metrics-on-virtual-machines)。
- 针对经典存储指标的经典警报规则。 请参阅[有关监视经典存储帐户的指导](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/)。
- 针对某些存储帐户指标的经典警报规则。 请参阅本文稍后所述的[详细信息](#storage-account-metrics)。

如果你的订阅包含任何此类经典规则，必须手动迁移。 由于我们无法提供自动迁移，任何此类现有经典指标警报会继续运行到 2020 年 6 月。 此缓冲期可让你从容地迁移到新警报。 但是，在 2019 年 6 月之后，不再可以创建新的经典警报。

### <a name="guest-metrics-on-virtual-machines"></a>虚拟机上的来宾指标

在针对来宾指标创建新的指标警报之前，必须先将来宾指标发送到 Azure Monitor 自定义指标存储。 遵照以下说明在诊断设置中启用 Azure Monitor 接收器：

- [为 Windows VM 启用来宾指标](collect-custom-metrics-guestos-resource-manager-vm.md)
- [为 Linux VM 启用来宾指标](collect-custom-metrics-linux-telegraf.md)

完成这些步骤后，可以针对来宾指标创建新的指标警报。 创建新的指标警报后，可以删除经典警报。

### <a name="storage-account-metrics"></a>存储帐户指标

可以迁移针对存储帐户的所有经典警报，但针对以下指标的警报除外：

- PercentAuthorizationError
- PercentClientOtherError
- PercentNetworkError
- PercentServerOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- AnonymousThrottlingError
- SASThrottlingError

必须基于[旧的和新的存储指标之间的映射](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics)迁移针对 Percent 指标的经典警报规则。 需要相应地修改阈值，因为提供的新指标是绝对指标。

必须将针对 AnonymousThrottlingError 和 SASThrottlingError 的经典警报规则拆分为两个新警报，因为没有任何合并的指标可提供相同的功能。 需要相应地调整阈值。

## <a name="rollout-phases"></a>推出阶段

迁移工具将分阶段推出到使用经典警报规则的客户。 当订阅准备好使用该工具迁移时，订阅所有者会收到一封电子邮件。

> [!NOTE]
> 由于该工具是分阶段推出的，在早期阶段，你可能会发现大部分订阅尚未做好迁移的准备。

目前有一部分订阅已标记为准备好迁移。 这些订阅包括仅针对以下资源类型应用经典警报规则的订阅。 后续阶段会添加对更多资源类型的支持。

- Microsoft.apimanagement/service
- Microsoft.batch/batchaccounts
- Microsoft.cache/redis
- Microsoft.datafactory/datafactories
- Microsoft.dbformysql/servers
- Microsoft.dbforpostgresql/servers
- Microsoft.eventhub/namespaces
- Microsoft.logic/workflows
- Microsoft.network/applicationgateways
- Microsoft.network/dnszones
- Microsoft.network/expressroutecircuits
- Microsoft.network/loadbalancers
- Microsoft.network/networkwatchers/connectionmonitors
- Microsoft.network/publicipaddresses
- Microsoft.network/trafficmanagerprofiles
- Microsoft.network/virtualnetworkgateways
- Microsoft.search/searchservices
- Microsoft.servicebus/namespaces
- Microsoft.streamanalytics/streamingjobs
- Microsoft.timeseriesinsights/environments
- Microsoft.web/hostingenvironments/workerpools
- Microsoft.web/serverfarms
- Microsoft.web/sites

## <a name="who-can-trigger-the-migration"></a>谁可以触发迁移？

在订阅级别拥有“监视参与者”内置角色的任何用户都可以触发迁移。 拥有自定义角色和以下权限的用户也可以触发迁移：

- */read
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*

## <a name="common-problems-and-remedies"></a>常见问题和补救措施

[触发迁移](alerts-using-migration-tool.md)后，提供的电子邮件地址会收到电子邮件，其中告知迁移已完成，或者需要你采取任何措施。 本部分描述了一些常见问题及其处理方法。

### <a name="validation-failed"></a>验证失败

由于最近对订阅中的经典警报规则做了某些更改，无法迁移该订阅。 此问题是暂时性的。 几天后当迁移状态恢复为“准备好迁移”时，可以重新开始迁移。 

### <a name="policy-or-scope-lock-preventing-us-from-migrating-your-rules"></a>策略或范围锁阻止我们迁移你的规则

在迁移过程中，将会创建新的指标警报和新的操作组，然后删除经典警报规则。 但是，有某个策略或范围锁阻止我们创建资源。 根据具体的策略或范围锁，无法迁移某些或所有规则。 暂时删除该范围锁或策略并再次触发迁移即可解决此问题。

## <a name="next-steps"></a>后续步骤

- [如何使用迁移工具](alerts-using-migration-tool.md)
- [准备迁移](alerts-prepare-migration.md)
