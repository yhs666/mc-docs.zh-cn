---
title: 排查将 Azure 资源移到新订阅或资源组时出现的错误
description: 使用 Azure Resource Manager 将资源移到新的资源组或订阅。
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: conceptual
origin.date: 08/19/2019
ms.date: 08/26/2019
ms.author: v-yeche
ms.openlocfilehash: 100b2679f71ac841d81e3eebbba8a4e5ce355fc5
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993569"
---
<!--Verified successfully-->
# <a name="troubleshoot-moving-azure-resources-to-new-resource-group-or-subscription"></a>排查将 Azure 资源移到新的资源组或订阅时遇到的问题

本文提供了一些建议来帮助解决在移动资源时遇到的问题。

## <a name="upgrade-a-subscription"></a>升级订阅

如果确实想要升级 Azure 订阅（例如从免费切换到标准的预先支付套餐），则需要转换订阅。

<!--MOONCAKE: CORRECT ON FOLLOWING UPGRADE SUBSCRIPTION-->

* 若要升级试用版，请参阅[将试用版或 Azure Imagine Azure 订阅升级为标准预付费套餐](/billing/billing-upgrade-1rmb-trial/)。
* 若要更改标准预付费套餐帐户，请参阅[将 Azure 标准预付费套餐订阅更改为其他套餐](/billing/billing-upgrade-to-ea)。

<!--MOONCAKE: CORRECT ON /billing/billing-upgrade-1rmb-trial AND /billing/billing-upgrade-to-ea-->

如果无法转换订阅，请[创建 Azure 支持请求](https://support.azure.cn/support/support-azure/?l=zh-cn)。 选择“订阅管理”  作为问题类型。

## <a name="service-limitations"></a>服务限制

有些服务在移动资源时需要注意额外的事项。 如果要移动以下服务，请务必查看指南和限制。

* [应用服务](./move-limitations/app-service-move-limitations.md)
    
    <!--Not Available on * [Azure DevOps Services](https://docs.microsoft.com/zh-cn/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)-->

* [经典部署模型](./move-limitations/classic-model-move-limitations.md)
* [联网](./move-limitations/networking-move-limitations.md)
* [恢复服务](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure-resource-manager/toc.json)
* [虚拟机](./move-limitations/virtual-machines-move-limitations.md)

## <a name="large-requests"></a>大型请求

在可能的情况下，将大型移动分为单独的移动操作。 在一次操作中有 800 多项资源时，资源管理器会立即返回错误。 但是，移动 800 项以下的资源也可能因超时而失败。

## <a name="resource-not-in-succeeded-state"></a>资源未处于成功状态

如果收到一条错误消息，指出由于资源未处于成功状态而无法移动资源，则它可能实际上是阻止移动的依赖资源。 请参阅[依赖资源的状态](./move-limitations/networking-move-limitations.md#state-of-dependent-resources)。

## <a name="next-steps"></a>后续步骤

有关用于移动资源的命令，请参阅[将资源移到新资源组或订阅](resource-group-move-resources.md)。

<!--Update_Description: wording update -->