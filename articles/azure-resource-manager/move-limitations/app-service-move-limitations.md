---
title: 将 Azure 应用服务资源移到新的订阅或资源组
description: 使用 Azure 资源管理器将应用服务资源移到新的资源组或订阅。
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: conceptual
origin.date: 07/09/2019
ms.date: 07/22/2019
ms.author: v-yeche
ms.openlocfilehash: 405480a5013a7edd4f5122e5bb3f55429270c496
ms.sourcegitcommit: 5fea6210f7456215f75a9b093393390d47c3c78d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68337565"
---
# <a name="move-guidance-for-app-service-resources"></a>针对应用服务资源的移动指南

移动应用服务资源的步骤各不相同，具体取决于是在订阅内移动资源，还是将资源移到新的订阅。

## <a name="move-in-same-subscription"></a>在同一订阅中移动

_在同一订阅中_移动 Web 应用时，无法移动第三方 SSL 证书。 不过，可以将 Web 应用移动到新的资源组而不移动其第三方证书，并且，应用的 SSL 功能仍然可以工作。

如果希望随 Web 应用移动 SSL 证书，请执行以下步骤：

1. 从 Web 应用中删除第三方证书，但保留证书的副本
2. 移动 Web 应用。
3. 将第三方证书上传到移动后的 Web 应用。

## <a name="move-across-subscriptions"></a>跨订阅移动

_在订阅之间_移动 Web 应用时存在以下限制：

- 目标资源组中不能有任何现有的应用服务资源。 应用服务资源包括：
    - Web 应用
    - 应用服务计划
    - 上传或导入的 SSL 证书
    - 应用服务环境
- 资源组中的所有应用服务资源必须一起移动。
- 只能从最初创建应用服务资源的资源组中移动它们。 如果应用服务资源不再位于其原始资源组中，请将其移回其原始资源组。 然后，在订阅之间移动资源。

<!--Pending for verify after tranlation-->

如果忘记了原始资源组，可以通过诊断来查找。 对于 Web 应用，请选择“诊断和解决问题”  。 然后，选择“配置和管理”。 

![选择诊断](./media/app-service-move-limitations/select-diagnostics.png)

选择“迁移选项”  。

![选择迁移选项](./media/app-service-move-limitations/select-migration.png)

选择通过建议的步骤来移动 Web 应用的选项。

![选择建议的步骤](./media/app-service-move-limitations/recommended-steps.png)

可以看到在移动资源之前需采取的建议操作。 该信息包含 Web 应用的原始资源组。

![建议](./media/app-service-move-limitations/recommendations.png)

<!--Pending for verify after tranlation-->

## <a name="move-app-service-certificate"></a>移动应用服务证书

可将应用服务证书移动到新的资源组或订阅。 如果应用服务证书已绑定到某个 Web 应用，必须先执行一些步骤，然后才能将资源移到新的订阅中。 移动资源之前，请在 Web 应用中删除 SSL 绑定和专用证书。 应用服务证书不需删除，只需删除 Web 应用中的专用证书。

## <a name="move-support"></a>移动支持

若要确定可以移动哪些应用服务资源，请查看以下项的移动支持状态：

- [Microsoft.AppService](../move-support-resources.md#microsoftappservice)
    <!--Not Available on - [Microsoft.CertificateRegistration](../move-support-resources.md#microsoftcertificateregistration)-->
    <!--Not Available on - [Microsoft.DomainRegistration](../move-support-resources.md#microsoftdomainregistration)-->
- [Microsoft.Web](../move-support-resources.md#microsoftweb)

## <a name="next-steps"></a>后续步骤

有关用于移动资源的命令，请参阅[将资源移到新资源组或订阅](../resource-group-move-resources.md)。

<!-- Update_Description: new articles on app service move limitations -->
<!--ms.date: 07/22/2019-->