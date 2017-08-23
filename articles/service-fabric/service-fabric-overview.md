---
title: "Azure 上的 Service Fabric 概述 | Azure"
description: "Service Fabric 概述，其中应用程序由许多微服务组成，以提供扩展性和恢复能力。 Service Fabric 是一个分布式系统平台，用于生成面向云的可缩放、可靠且易于管理的应用程序。"
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: masnider
ms.assetid: bbcc652a-a790-4bc4-926b-e8cd966587c0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 07/02/2017
ms.date: 08/21/2017
ms.author: v-yeche
ms.openlocfilehash: 6e1d6715a513db9dd3de5a83e358f9f9330c1b9a
ms.sourcegitcommit: ece23dc9b4116d07cac4aaaa055290c660dc9dec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/17/2017
---
# <a name="overview-of-azure-service-fabric"></a>Azure Service Fabric 概述
Azure Service Fabric 是一种分布式系统平台，可借助它轻松打包、部署和管理可缩放且可靠的微服务和容器。 Service Fabric 还解决了开发和管理云本机应用程序面临的重大难题。 开发人员和管理员不仅可以避免复杂的基础结构问题，而且可以专注于实现可缩放、可靠且可管理的要求苛刻的任务关键型工作负荷。 Service Fabric 代表了下一代平台，用于生成和管理在容器中运行的企业级单层云规模应用程序。
<!-- Not Available -- Channel9 video -- >

## Applications composed of microservices 
Service Fabric enables you to build and manage scalable and reliable applications composed of microservices that run at high density on a shared pool of machines, which is referred to as a cluster. It provides a sophisticated, lightweight runtime to build distributed, scalable, stateless, and stateful microservices running in containers. It also provides comprehensive application management capabilities to provision, deploy, monitor, upgrade/patch, and delete deployed applications including containerized services.

Service Fabric powers many Microsoft services today, including Azure SQL Database, Azure Cosmos DB, Cortana, Microsoft Power BI, Microsoft Intune, Azure Event Hubs, Azure IoT Hub, Dynamics 365, Skype for Business, and many core Azure services.

Service Fabric is tailored to create cloud native services that can start small, as needed, and grow to massive scale with hundreds or thousands of machines.

Today's Internet-scale services are built of microservices. Examples of microservices include protocol gateways, user profiles, shopping carts, inventory processing, queues, and caches. Service Fabric is a microservices platform that gives every microservice (or container) a unique name that can be either stateless or stateful.

Service Fabric provides comprehensive runtime and lifecycle management capabilities to applications that are composed of these microservices. It hosts microservices inside containers that are deployed and activated across the Service Fabric cluster. A move from virtual machines to containers makes possible an order-of-magnitude increase in density. Similarly, another order of magnitude in density becomes possible when you move from containers to microservices in these containers. For example, a single cluster for Azure SQL Database comprises hundreds of machines running tens of thousands of containers that host a total of hundreds of thousands of databases. Each database is a Service Fabric stateful microservice. 

For more on the microservices approach, read [Why a microservices approach to building applications?](service-fabric-overview-microservices.md)

## Container deployment and orchestration
Service Fabric is an [container orchestrator](service-fabric-cluster-resource-manager-introduction.md) deploying microservices across a cluster of machines. Microservices can be developed in many ways from using the [Service Fabric programming models ](service-fabric-choose-framework.md), [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md),to deploying [any code of your choice](service-fabric-deploy-existing-app.md)Importantly, you can mix both services in processes and services in containers in the same application. If you just want to [deploy and manage containers](service-fabric-containers-overview.md), Service Fabric is a perfect choice as a container orchestrator.

## Any OS, any cloud
Service Fabric runs everywhere. You can create clusters for Service Fabric in many environments, including Azure or on premises, on Windows Server, or on Linux. You can even create clusters on other public clouds. In addition, the development environment in the SDK is **identical** to the production environment, with no emulators involved. In other words, what runs on your local development cluster deploys to the clusters in other environments.

![Service Fabric platform][Image1]

For more information on creating clusters on-premises, read [creating a cluster on Windows Server or Linux](service-fabric-deploy-anywhere.md) or for Azure creating a cluster [via the Azure portal](service-fabric-cluster-creation-via-portal.md).

## Stateless and stateful microservices for Service Fabric
Service Fabric enables you to build applications that consist of microservices or containers. Stateless microservices (such as protocol gateways and web proxies) do not maintain a mutable state outside a request and its response from the service. Azure Cloud Services worker roles are an example of a stateless service. Stateful microservices (such as user accounts, databases, devices, shopping carts, and queues) maintain a mutable, authoritative state beyond the request and its response. Today's Internet-scale applications consist of a combination of stateless and stateful microservices. 

A key differentation with Service Fabric is its strong focus on building stateful services, either with the [built-in programming models ](service-fabric-choose-framework.md) or with  containerized stateful services. The [application scenarios](service-fabric-application-scenarios.md) describe the scenarios where stateful services are used.

## Application lifecycle management
Service Fabric provides support for the full application lifecycle and CI/CD of cloud applications including containers. This lifecycle includes development through deployment, daily management, and maintenance to eventual decommissioning.

Service Fabric application lifecycle management capabilities enable application administrators and IT operators to use simple, low-touch workflows to provision, deploy, patch, and monitor applications. These built-in workflows greatly reduce the burden on IT operators to keep applications continuously available.

Most applications consist of a combination of stateless and stateful microservices, containers, and other executables that are deployed together. By having strong types on the applications, Service Fabric enables the deployment of multiple application instances. Each instance is managed and upgraded independently. Importantly, Service Fabric can deploy containers or any executables and make them reliable. For example, Service Fabric can deploy .NET, ASP.NET Core, node.js, Windows containers, Linux containers, Java virtual machines, scripts, Angular, or literally anything that makes up your application.

Service Fabric is integrated with CI/CD tools such as [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [Jenkins](https://jenkins.io/index.html), and [Octopus Deploy](https://octopus.com/) and can be used with any other popular CI/CD tool.

For more information about application lifecycle management, read [Application lifecycle](service-fabric-application-lifecycle.md). For more about how to deploy any code, see [deploy a guest executable](service-fabric-deploy-existing-app.md).

## Key capabilities
By using Service Fabric, you can:

* Deploy to Azure or to on-premises datacenters that run Windows or Linux with zero code changes. Write once, and then deploy anywhere to any Service Fabric cluster.
* Develop scalable applications that are composed of microservices by using the Service Fabric programming models, containers, or any code.
* Develop highly reliable stateless and stateful microservices. Simplify the design of your application by using stateful microservices. 
* Use the novel Reliable Actors programming model to create cloud objects with self contained code and state.
* Deploy and orchestrate containers that include Windows containers and Linux containers. Service Fabric is a data aware, stateful, container orchestrator.
* Deploy applications in seconds, at high density with hundreds or thousands of applications or containers per machine.
* Deploy different versions of the same application side by side, and upgrade each application independently.
* Manage the lifecycle of your applications without any downtime, including breaking and nonbreaking upgrades.
* Scale out or scale in the number of nodes in a cluster. As you scale nodes, your applications automatically scale.
* Monitor and diagnose the health of your applications and set policies for performing automatic repairs.
* Watch the resource balancer orchestrate the redistribution of applications across the cluster. Service Fabric recovers from failures and optimizes the distribution of load based on available resources.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>后续步骤
* 更多相关信息：
  * [为什么要使用微服务方法构建应用程序？](service-fabric-overview-microservices.md)
  * [术语概述](service-fabric-technical-overview.md)
* 设置 Service Fabric [开发环境](service-fabric-get-started.md)  
* 了解 [Service Fabric 支持选项](service-fabric-support.md)

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png

<!--Update_Description: update meta properties, wording update-->