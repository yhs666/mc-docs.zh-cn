---
title: 应用服务环境的自定义设置 - Azure
description: 应用服务环境的自定义配置设置
services: app-service
documentationcenter: ''
author: stefsch
manager: nirma
editor: ''
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: tutorial
origin.date: 01/16/2018
ms.date: 09/20/2019
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: 0456d2c5a092bdf244452ec93fabc6632b4eebef
ms.sourcegitcommit: 97fa37512f79417ff8cd86e76fe62bac5d24a1bd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2019
ms.locfileid: "73041151"
---
# <a name="custom-configuration-settings-for-app-service-environments"></a>应用服务环境的自定义配置设置
## <a name="overview"></a>概述
由于应用服务环境 (ASE) 对单个客户是隔离的，因此有一些可专门应用于应用服务环境的配置设置。 本文介绍各种可用于应用服务环境的特定自定义设置。

如果没有应用服务环境，请参阅 [How to Create an App Service Environment](create-external-ase.md)（如何创建应用服务环境）。

可以在新的 **clusterSettings** 属性中使用数组存储应用服务环境自定义设置。 可以在 *hostingEnvironments* Azure Resource Manager 实体的“Properties”字典中找到此属性。

以下简略的 Resource Manager 模板代码段显示了 **clusterSettings** 属性：

    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

**clusterSettings** 属性可以包含在 Resource Manager 模板中，以更新应用服务环境。

<!-- Azure Resource Explorer not available -->

## <a name="disable-tls-10-and-tls-11"></a>禁用 TLS 1.0 和 TLS 1.1

若要逐个应用地管理 TLS 设置，则可按[实施 TLS 设置](https://docs.azure.cn/app-service/app-service-web-tutorial-custom-ssl#enforce-tls-versions)文档提供的指南进行操作。 

对于 ASE 中的所有应用，若要禁用所有入站 TLS 1.0 和 TLS 1.1 流量，可以设置以下 **clusterSettings** 条目：

    "clusterSettings": [
        {
            "name": "DisableTls1.0",
            "value": "1"
        }
    ],

设置的名称显示 1.0，但在配置以后，却禁用了 TLS 1.0 和 TLS 1.1。

## <a name="change-tls-cipher-suite-order"></a>更改 TLS 密码套件顺序
<!-- link is 404 -->
来自客户的另一个问题是，他们是否可以修改由其服务器协商的密码列表，而这可以通过修改 **clusterSettings** 来实现，如下所示。

    "clusterSettings": [
        {
            "name": "FrontEndSSLCipherSuiteOrder",
            "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
        }
    ],

> [!WARNING]
> 如果对 SChannel 无法理解的密码套件设置不正确的值，与服务器的所有 TLS 通信可能会停止运行。 在这种情况下，必须从 *clusterSettings* 中删除 **FrontEndSSLCipherSuiteOrder** 条目，并提交更新的 Resource Manager 模板以还原回默认的密码套件设置。  请谨慎使用此功能。
> 

## <a name="get-started"></a>入门
Azure 快速入门 Resource Manager 模板站点包含具有[创建应用服务环境](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/)基本定义的模板。

<!-- LINKS -->

<!-- IMAGES -->
