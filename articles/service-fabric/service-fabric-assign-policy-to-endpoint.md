---
title: 将访问策略分配到 Azure Service Fabric 服务终结点 | Azure
description: 了解如何将安全性访问策略分配给 Service Fabric 服务中的 HTTP 或 HTTPS 终结点。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 03/21/2018
ms.date: 05/28/2018
ms.author: v-yeche
ms.openlocfilehash: 347373580962aaffe8426dc7455651fb376a95e1
ms.sourcegitcommit: e50f668257c023ca59d7a1df9f1fe02a51757719
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/26/2018
ms.locfileid: "34554249"
---
# <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>为 HTTP 和 HTTPS 终结点分配安全访问策略
如果将运行方式策略应用到服务，且服务清单声明使用 HTTP 协议的终结点资源，则必须指定 SecurityAccessPolicy。  SecurityAccessPolicy 会确保分配给这些终结点的端口已正确限制为运行该服务所用的用户帐户。 否则，**http.sys** 无法访问服务，并且无法从客户端调用。 以下示例将 Customer1 帐户应用于名为 **EndpointName** 的终结点，并向它授予完全访问权限。

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

针对 HTTPS 终结点，还要指出要返回给客户端的证书名称。 使用 EndpointBindingPolicy 引用证书。  应用程序清单的“证书”部分中定义了证书。

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
有关后续步骤，请阅读以下文章：
* [了解应用程序模型](service-fabric-application-model.md)
* [在服务清单中指定资源](service-fabric-service-manifest-resources.md)
* [部署应用程序](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
<!-- Update_Description: update meta properties -->
