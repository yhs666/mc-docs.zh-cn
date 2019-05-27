---
title: Azure API 管理层的基于功能的比较 | Microsoft Docs
description: 本文根据各个 API 管理层提供的功能对这些层进行了比较。
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 11/26/2018
ms.date: 06/03/2019
ms.author: v-yiso
ms.openlocfilehash: ae49699e8b86f475ae538d5bad74eb70c8be2a6b
ms.sourcegitcommit: 5a57f99d978b78c1986c251724b1b04178c12d8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66194997"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Azure API 管理层的基于功能的比较

每个 API 管理[定价层](https://www.azure.cn/zh-cn/pricing/details/api-management/)都提供了一组不同的功能和每单位[容量](api-management-capacity.md)。 下表总结了每个层中提供的主要功能。 某些功能可能根据层以不同的方式工作或具有不同的能力。 在这种情况下，介绍这些功能的文档文章中指出了差异。

| 功能                                                                                      | 开发人员      | 基本          | 标准       | 高级        |
| -------------------------------------------------------------------------------------------- | -------------- | -------------- | -------------- | -------------- |
| Azure AD 集成<sup>1</sup>                                                             | 是            | 否             | 是            | 是            |
| 虚拟网络 (VNet) 支持                                                               | 是            | 否             | 否             | 是            |
| 多区域部署                                                                      | 否             | 否             | 否             | 是            |
| 多个自定义域名                                                                 | 否             | 否             | 否             | 是            |
| 开发人员门户<sup>2</sup>                                                                 | 是            | 是            | 是            | 是            |
| 内置缓存                                                                               | 是            | 是            | 是            | 是            |
| 内置分析                                                                           | 是            | 是            | 是            | 是            |
| [SSL 设置](api-management-howto-manage-protocols-ciphers.md)                             | 是            | 是            | 是            | 是            |
| [外部缓存](https://aka.ms/apimbyoc)                                                    | 是            | 是            | 是            | 是            |
| [客户端证书身份验证](api-management-howto-mutual-certificates-for-clients.md) | 是            | 是            | 是            | 是            |
| [备份和还原](api-management-howto-disaster-recovery-backup-restore.md)               | 是            | 是            | 是            | 是            |
| [基于 Git 的管理](api-management-configuration-repository-git.md)                        | 是            | 是            | 是            | 是            |
| 直接管理 API                                                                        | 是            | 是            | 是            | 是            |
| Azure Monitor 日志和指标                                                               | 是            | 是            | 是            | 是            |

<sup>1</sup> 允许使用 Azure AD 作为标识提供者，以用于开发人员门户上的用户登录。<br/>
<sup>2</sup> 包括相关功能，例如用户、组、问题、应用程序和电子邮件模板以及通知。<br/>

