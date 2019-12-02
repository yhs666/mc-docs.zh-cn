---
title: 移动 Azure 应用服务资源
description: 使用 Azure 资源管理器将应用服务资源移到新的资源组或订阅。
ms.topic: conceptual
origin.date: 07/09/2019
ms.date: 11/25/2019
ms.openlocfilehash: badfefa39ff418d87a9418be22a2b80980d954a8
ms.sourcegitcommit: 9e92bcf6aa02fc9e7b3a29abadf6b6d1a8ece8c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74389468"
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
<!--MOONCAKE: Not Available on **Diagnose and solve problems**-->
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

<!-- Update_Description: update meta properties, wording update -->