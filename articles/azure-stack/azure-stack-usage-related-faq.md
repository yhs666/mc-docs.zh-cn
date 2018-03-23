---
title: 用量 API 相关的常见问题解答 | Microsoft Docs
description: Azure Stack 计量器列表、与 Azure 用量 API 的比较、使用时间和报告时间、错误代码。
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/30/2018
ms.date: 03/04/2018
ms.author: v-junlch
ms.openlocfilehash: a884deb49f553d2b83fe9c4096b8070a181bdd0b
ms.sourcegitcommit: 9b5cc262f13a0fc9e0fd9495e3fbb6f394ba1812
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Azure Stack 用量 API 的常见问题解答
此文解答有关 Azure Stack 用量 API 的一些常见问题。

## <a name="what-meter-ids-can-i-see"></a>可以查看哪些计量 ID？
系统针对以下资源提供程序报告用量：

| **资源提供程序** | **电表 ID** | **计量器名称** | **单位** | 其他信息 |
| --- | --- | --- | --- | --- |
| **网络** |F271A8A388C44D93956A063E1D2FA80B |静态 IP 地址用量 |IP 地址| 已用 IP 地址计数。 如果以每日粒度调用用量 API，计量器将返回 IP 地址乘以小时数。 |
| |9E2739BA86744796B465F64674B822BA |动态 IP 地址用量 |IP 地址| 已用 IP 地址计数。 如果以每日粒度调用用量 API，计量器将返回 IP 地址乘以小时数。 |
| **存储** |B4438D5D-453B-4EE1-B42A-DC72E377F1E4 |TableCapacity |GB\*小时 |表使用的总容量 |
| |B5C15376-6C94-4FDD-B655-1A69D138ACA3 |PageBlobCapacity |GB\*小时 |页 Blob 使用的总容量 |
| |B03C6AE7-B080-4BFA-84A3-22C800F315C6 |QueueCapacity |GB\*小时 |队列使用的总容量 |
| |09F8879E-87E9-4305-A572-4B7BE209F857 |BlockBlobCapacity |GB\*小时 |块 Blob 使用的总容量 |
| |B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 |TableTransactions |请求计数（以万为单位） |表服务请求数（以万为单位） |
| |50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D |TableDataTransIn |传入数据 (GB) |表服务传入数据 (GB) |
| |1B8C1DEC-EE42-414B-AA36-6229CF199370 |TableDataTransOut |传出数据 (GB) |表服务传出数据 (GB) |
| |43DAF82B-4618-444A-B994-40C23F7CD438 |BlobTransactions |请求计数（以万为单位） |Blob 服务请求数（以万为单位） |
| |9764F92C-E44A-498E-8DC1-AAD66587A810 |BlobDataTransIn |传入数据 (GB) |Blob 服务传入数据 (GB) |
| |3023FEF4-ECA5-4D7B-87B3-CFBC061931E8 |BlobDataTransOut |传出数据 (GB) |Blob 服务传出数据 (GB) |
| |EB43DD12-1AA6-4C4B-872C-FAF15A6785EA |QueueTransactions |请求计数（以万为单位） |队列服务请求数（以万为单位） |
| |E518E809-E369-4A45-9274-2017B29FFF25 |QueueDataTransIn |传入数据 (GB) |队列服务传入数据 (GB) |
| |DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2 |QueueDataTransOut |传出数据 (GB) |队列服务传出数据 (GB) |
| **Sql RP**            | CBCFEF9A-B91F-4597-A4D3-01FE334BED82 | DatabaseSizeHourSqlMeter   | MB\*小时   | 创建时的总数据库容量。 如果以每日粒度调用用量 API，计量器将返回 MB 乘以小时数。 |
| **MySql RP**          | E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3 | DatabaseSizeHourMySqlMeter | MB\*小时    | 创建时的总数据库容量。 如果以每日粒度调用用量 API，计量器将返回 MB 乘以小时数。 |
| **计算** |FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5 |基本 VM 大小小时 |虚拟核心小时数 | 虚拟核心数乘以 VM 运行小时数 |
| |9CD92D4C-BAFD-4492-B278-BEDC2DE8232A |Windows VM 大小小时数 |虚拟核心小时数 | 虚拟核心数乘以 VM 运行小时数 |
| |6DAB500F-A4FD-49C4-956D-229BB9C8C793 |VM 大小小时数 |VM 小时数 |捕获基本 VM 和 Windows 的 VM。 不针对核心调整 |
| **密钥保管库** |EBF13B9F-B3EA-46FE-BF54-396E93D48AB4 |Key Vault 事务 | 请求计数（以万为单位）| Key Vault 接收的 REST API 请求数 |
| **应用服务** |190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  | 应用服务   | 虚拟核心小时数  | 用于运行应用服务的虚拟核心数 |
|             | 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE | Functions - 计算请求数      | 10 个请求              | 适用于 Functions  |
|             | 957E9F36-2C14-45A1-B6A1-1723EF71A01D | 共享应用服务小时数          | 1 小时                   |                       |
|             | 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9 | 免费应用服务小时数            | 1 小时                   |                       |
|             | 88039D51-A206-3A89-E9DE-C5117E2D10A6 | 小型标准应用服务小时数  | 1 小时                   |                       |
|             | 83A2A13E-4788-78DD-5D55-2831B68ED825 | 中型标准应用服务小时数 | 1 小时                   |                       |
|             | 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6 | 大型标准应用服务小时数  | 1 小时                   |                       |
|             | 264ACB47-AD38-47F8-ADD3-47F01DC4F473 | SNI SSL                           | 按 SNI SSL 绑定      | 适用于应用服务 |
|             | 60B42D72-DC1C-472C-9895-6C516277EDB4 | IP SSL                            | 按基于 IP 的 SSL 绑定 | 适用于应用服务 |

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Azure Stack 用量 API 与 [Azure 用量 API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)（目前为公共预览版）有何差别？
- 租户用量 API 与 Azure API 相同，但有一点除外：Azure Stack 目前不支持 *showDetails* 标志。
- 提供程序用量 API 只适用于 Azure Stack。
- 目前，Azure Stack 不提供 Azure 中所提供的[费率卡 API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx)。

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>使用时间与报告时间有何差别？
用量数据报告包含两个主要时间值：

- **报告时间**。 用量事件进入用量系统的时间
- **使用时间**。 使用 Azure Stack 资源的时间

你可能会发现，特定用量事件的“使用时间”与“报告时间”值有差异。 在任何环境中，延迟可能长达数小时。

目前，只能按“报告时间”查询。

## <a name="what-do-these-usage-api-error-codes-mean"></a>这些用量 API 错误代码的含义是什么？
| **HTTP 状态代码** | 错误代码 | **说明** |
| --- | --- | --- |
| 400/错误的请求 |*NoApiVersion* |未提供 *api-version* 查询参数。 |
| 400/错误的请求 |*InvalidProperty* |属性缺失或使用了无效值。 响应正文中错误代码内的消息指示缺少属性。 |
| 400/错误的请求 |*RequestEndTimeIsInFuture* |*ReportedEndTime* 的值是将来时间。 此参数不允许将来的时间值。 |
| 400/错误的请求 |*SubscriberIdIsNotDirectTenant* |提供程序 API 调用使用的订阅 ID 不是调用方的有效租户。 |
| 400/错误的请求 |*SubscriptionIdMissingInRequest* |缺少调用方的订阅 ID。 |
| 400/错误的请求 |*InvalidAggregationGranularity* |请求的聚合粒度无效。 有效值为 daily 和 hourly。 |
| 503 |*ServiceUnavailable* |由于服务繁忙或调用受到限制，发生了可重试的错误。 |

## <a name="next-steps"></a>后续步骤
[Azure Stack 中的客户计费和退款](azure-stack-billing-and-chargeback.md)

[提供程序资源使用情况 API](azure-stack-provider-resource-api.md)

[租户资源使用情况 API](azure-stack-tenant-resource-usage-api.md)

