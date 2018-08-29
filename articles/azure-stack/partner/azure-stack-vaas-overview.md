---
title: 适用于 Azure Stack 的验证即服务概述 | Azure
description: 概述了 Azure Stack 验证即服务的常见问题。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 07/24/2018
ms.date: 08/27/2018
ms.author: v-jay
ms.reviewer: johnhas
ms.openlocfilehash: 45c4726d266302ab63107b0168cacc16cd772d2a
ms.sourcegitcommit: 9dda276bc6675d7da3070ea6145079f1538588ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "42869684"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>什么是适用于 Azure Stack 的验证即服务？

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

验证即服务 (VaaS) 是一项原生 Azure 服务，它适用于与 Azure 联合对 Azure Stack 产品/服务进行工程设计的解决方案合作伙伴。 解决方案合作伙伴可以使用该服务检查其解决方案是否满足 Azure 的要求以及是否可以按预期与 Azure Stack 配合工作。

VaaS 的主要用途如下：

- 验证新的 Azure Stack 解决方案
- 验证对 Azure Stack 软件所做的更改
- 获取在部署期间使用的进行了数字签名的解决方案合作伙伴程序包
- 预览 Azure Stack 验证辅助材料

## <a name="validate-new-azure-stack-solution"></a>验证新的 Azure Stack 解决方案

合作伙伴使用解决方案验证工作流来检查新的 Azure Stack 解决方案。 解决方案必须通过必需的硬件实验室工具包 (HKL) Azure Stack 测试组件测试。 你只需要为每个新解决方案运行两次工作流：针对最低配置和最高配置各运行一次。

有关详细信息，请参阅[验证新的 Azure Stack 解决方案](azure-stack-vaas-validate-solution-new.md)。

## <a name="validate-changes-to-the-azure-stack-software"></a>验证对 Azure Stack 软件所做的更改

合作伙伴使用程序包验证工作流检查他们的解决方案是否可以与最新的 Azure Stack 软件更新配合工作。 至少必须在 Azure 建议的硬件环境中与在使用修补和更新 (P&U) 来应用更新的解决方案中运行程序包验证工作流。 应当在基线内部版本中运行程序包验证。

有关详细信息，请参阅[验证来自 Microsoft 的软件更新](azure-stack-vaas-validate-microsoft-updates.md)。

## <a name="get-digitally-signed-solution-partner-packages"></a>获取进行了数字签名的解决方案合作伙伴程序包

除了验证 Azure Stack 更新之外，还可以使用程序包验证工作流检查对 OEM 自定义程序包的更新，这包括在部署 Azure Stack 软件期间使用的 Azure Stack 合作伙伴特定驱动程序、固件以及其他软件。 至少使用将受支持的最小规模的解决方案部署你在当前版本的 Azure Stack 软件上检查的程序包。 在开始测试之前，必须将更新后的程序包上传到服务。 如果测试成功，则通知 vaashelp@microsoft.com。 告诉 Azure Stack 合作伙伴，程序包已完成测试并且应当通过 Azure Stack 数字签名进行数字签名。 Azure 对程序包进行签名并通知 Azure Stack 合作伙伴，指出程序包已可从门户中下载。

有关详细信息，请参阅[验证 OEM 程序包](azure-stack-vaas-validate-oem-package.md)。

## <a name="preview-azure-stack-validation-collateral"></a>预览 Azure Stack 验证辅助材料

Azure 定期在 Azure Stack 中提供新功能。 作为向市场提供这些功能的开发流程的一部分，在测试轮次工作流中将提供新的测试辅助材料。 测试轮次工作流包括了来自其他工作流的测试辅助材料，可用于执行非正式的测试。 不要使用测试轮次工作流来提交结果供审批。 请使用解决方案验证和程序包验证工作流来使你的解决方案获得正式批准。

## <a name="next-steps"></a>后续步骤

- 首先[设置验证即服务帐户](azure-stack-vaas-validate-solution-new.md)
