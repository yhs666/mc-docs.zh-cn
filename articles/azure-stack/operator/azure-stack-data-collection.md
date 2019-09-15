---
title: Azure Stack 日志和数据处理 | Microsoft Docs
description: 了解 Azure Stack 如何收集信息。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 06/10/2019
ms.date: 09/16/2019
ms.author: v-jay
ms.reviewer: chengwei
ms.lastreviewed: 06/10/2019
ms.openlocfilehash: 70396aab9c33653e4a9a0999e19504a794b26f3f
ms.sourcegitcommit: 843028f54c4d75eba720ac8874562ab2250d5f4d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70857045"
---
# <a name="azure-stack-log-and-customer-data-handling"></a>Azure Stack 日志和客户数据处理 
*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*  

在某种程度上 Azure 是 Azure Stack 相关个人数据的处理方或辅助处理方，Azure 对所有客户承诺，(a) [联机服务条款](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)的“保护数据条款”部分中的“个人数据的处理；GDPR”条款和 (b) [联机服务条款](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)附件 4 中的“欧盟一般数据保护条例条款”从 2018 年 5 月 25 日开始生效。 

由于 Azure Stack 驻留在客户数据中心，因此，对于通过[诊断](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep)、[遥测](azure-stack-telemetry.md)和[计费](azure-stack-usage-reporting.md)与 Azure 共享的数据，Azure 是唯一的数据控制方。  

## <a name="data-access-controls"></a>数据访问控制 
Azure 员工受派调查特定的支持案例时，将获得加密数据的只读访问权限。 如果需要，Azure 员工还有权访问用于删除数据的工具。 对客户数据的所有访问都会受到审核和记录。  

数据访问控制：
- 在结案后，数据最多只会保留 90 天。
- 在 90 天期限内，客户随时可以删除数据。
- Azure 员工获得的数据访问权限根据不同的案例而定，并且只在需要帮助解决支持问题时才能获得该访问权限。 
- 如果 Azure 必须与 OEM 合作伙伴共享客户数据，必须经得客户的同意。  

### <a name="what-data-subject-requests-dsr-controls-do-customers-have"></a>客户可以实施哪些数据主题请求 (DSR) 控制？
如前所述，Azure 根据客户请求提供按需删除数据的支持。 客户可以请求我们的支持工程师在客户选择的任何时间删除给定案例的所有日志，然后再将数据永久擦除。  

### <a name="does-azure-notify-customers-when-the-data-is-deleted"></a>删除数据时，Azure 是否通知客户？
对于自动化数据删除操作（案例关闭后 90 天），我们不会主动联系客户并通知他们有关删除的信息。 

对于按需删除数据操作，Azure 支持工程师有权访问相应的工具进行按需数据删除，且在完成删除后可以通过电话向客户提供确认。

## <a name="diagnostic-data"></a>诊断数据
在支持过程中，Azure Stack 操作员可与 Azure Stack 支持和工程团队[共享诊断日志](azure-stack-configure-on-demand-diagnostic-log-collection.md#using-pep)，以方便进行故障排除。

Azure 为客户提供所需的工具和脚本用于收集及上传请求的诊断日志文件。 收集日志文件后，这些文件将通过 HTTPS 保护的加密连接发送到 Azure。 由于 HTTPS 提供在线加密，因此传输中加密无需密码。 Azure 收到日志后，会加密并存储日志，在关闭支持案例 90 天后自动将其删除。

## <a name="telemetry-data"></a>遥测数据
[Azure Stack 遥测](azure-stack-telemetry.md)通过互连用户体验将系统数据自动上传到 Azure。 Azure Stack 操作员随时可以通过相应的控制机制自定义遥测功能和隐私设置。

Azure 无意收集敏感数据，例如信用卡号、用户名和密码、电子邮件地址或类似的敏感信息。 如果我们确定敏感信息是无意中收集到的，我们会予以删除。 

## <a name="billing-data"></a>账单数据
[Azure Stack 计费](azure-stack-usage-reporting.md)利用 Azure 的计费和用量管道，因此遵循 Azure 合规性准则。

Azure Stack 操作员可以配置 Azure Stack，以将用量信息转发到 Azure 进行计费。 选择即用即付计费模型的多节点 Azure Stack 客户一定要这样做。 用量报告通过遥测单独进行控制，选择容量模式的多节点 Azure Stack 客户或 Azure Stack 开发工具包用户无需使用此功能。 对于上述方案，可以使用[注册脚本](azure-stack-usage-reporting.md)来禁用用量报告。


## <a name="next-steps"></a>后续步骤 
[详细了解 Azure Stack 安全性](azure-stack-security-foundations.md) 
