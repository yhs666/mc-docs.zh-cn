---
title: "描述 Azure Service Fabric 应用和服务 | Azure"
description: "介绍如何使用清单来描述 Service Fabric 应用程序和服务。"
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 12/07/2017
ms.date: 01/01/2018
ms.author: v-yeche
ms.openlocfilehash: 58fa191eb15a4d8751b03672a4e476fdfc5bc80a
ms.sourcegitcommit: 90e4b45b6c650affdf9d62aeefdd72c5a8a56793
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2017
---
# <a name="service-fabric-application-and-service-manifests"></a>Service Fabric 应用程序和服务清单
本文介绍如何使用 ApplicationManifest.xml 和 ServiceManifest.xml 文件来定义 Service Fabric 应用程序与服务并对其进行版本控制。  [ServiceFabricServiceModel.xsd 架构文档](service-fabric-service-model-schema.md)中阐述了这些清单文件的 XML 架构。

## <a name="describe-a-service-in-servicemanifestxml"></a>在 ServiceManifest.xml 中描述服务
服务清单以声明方式定义服务类型和版本。 它指定服务元数据，例如服务类型、运行状况属性、负载均衡度量值、服务二进制文件和配置文件。  换言之，它描述了组成一个服务包以支持一个或多个服务类型的代码、配置和数据包。 服务清单可以包含多个代码、配置和数据包，可单独对这些包进行版本控制。 下面是[投票示例应用程序](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart)的 ASP.NET Core Web 前端服务的服务清单：

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

**Version** 特性是未结构化的字符串，并且不由系统进行分析。 版本特性用于对每个组件进行版本控制，以进行升级。

**ServiceTypes** 声明此清单中的 **CodePackages** 支持哪些服务类型。 当一种服务针对这些服务类型之一进行实例化时，可激活此清单中声明的所有代码包，方法是运行这些代码包的入口点。 生成的进程应在运行时注册所支持的服务类型。 在清单级别而不是代码包级别声明服务类型。 因此，当存在多个代码包时，每当系统查找任何一种声明的服务类型时，都会激活所有代码包。

**EntryPoint** 指定的可执行文件通常是长时间运行的服务主机。 **SetupEntryPoint** 是特权入口点，以与 Service Fabric（通常是 *LocalSystem* 帐户）相同的凭据先于任何其他入口点运行。  提供单独的设置入口点可避免长时间使用高特权运行服务主机。 由 **EntryPoint** 指定的可执行文件在 **SetupEntryPoint** 成功退出后运行。 如果进程总是终止或出现故障，则监视并重启所产生的过程（再次从 **SetupEntryPoint** 开始）。  

**SetupEntryPoint** 的典型使用场景是在服务启动之前运行可执行文件，或使用提升的权限来执行操作时。 例如：

* 设置和初始化服务可执行文件所需的环境变量。 这并不限于通过 Service Fabric 编程模型编写的可执行文件。 例如，npm.exe 需要配置一些环境变量来部署 node.js 应用程序。
* 通过安装安全证书设置访问控制。

有关如何配置 **SetupEntryPoint** 的详细信息，请参阅[配置服务设置入口点的策略](service-fabric-application-runas-security.md)

**EnvironmentVariables**（在前面的示例中未设置）提供为此代码包设置的环境变量列表。 环境变量可以在 `ApplicationManifest.xml` 中重写，以便为不同的服务实例提供不同的值。 

**DataPackage**（在前面的示例中未设置）声明一个由 **Name** 特性命名的文件夹，该文件夹中包含进程将在运行时使用的静态数据。

**ConfigPackage** 声明一个由 **Name** 特性命名的文件夹，该文件夹中包含 *Settings.xml* 文件。 此设置文件包含用户定义的键值对设置部分，进程可在运行时读回这些设置。 升级期间，如果仅更改了 **ConfigPackage** **版本**，则不重启正在运行的进程。 相反，回调会向进程通知配置设置已更改，以便可以重新动态加载这些设置。 下面是 *Settings.xml* 文件的一个示例：

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

在不更改已编译的代码的情况下，要声明/更改的、由服务使用的**资源**，例如终结点。  可以通过应用程序清单中的 **SecurityGroup** 控制对服务清单中指定资源的访问。  在服务清单中定义了**终结点**资源时，如果未显式指定端口，则 Service Fabric 从保留的应用程序端口范围中分配端口。  详细了解如何[指定或重写终结点资源](service-fabric-service-manifest-resources.md)。

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application-in-applicationmanifestxml"></a>在 ApplicationManifest.xml 中描述应用程序
应用程序清单以声明方式描述应用程序类型和版本。 它指定服务组合元数据（如稳定名称、分区方案、实例计数/复制因子、安全/隔离策略、布置约束、配置替代和成分服务类型）。 此外还描述用于容纳应用程序的负载均衡域。

因此，应用程序清单在应用程序级别描述元素，并引用一个或多个服务清单，以组成应用程序类型。 下面是[投票示例应用程序](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart)的应用程序清单：

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

类似于服务清单， **Version** 特性是未结构化的字符串，并且不由系统进行分析。 版本特性也用于对每个组件进行版本控制，以进行升级。

**Parameters** 定义整个应用程序清单中使用的参数。 当应用程序已实例化并可以重写应用程序或服务配置设置时，可以提供这些参数的值。  如果在应用程序实例化期间未更改值，则使用默认参数值。 若要了解如何维护不同的应用程序和用于单个环境的服务参数，请参阅[管理多个环境的应用程序参数](service-fabric-manage-multiple-environment-app-configuration.md)。

**ServiceManifestImport** 包含对组成此应用程序类型的服务清单的引用。 一个应用程序清单可以包含多个服务清单导入，可单独对每个导入进行版本控制。 导入的服务清单将确定此应用程序类型中有效的服务类型。 在 ServiceManifestImport 中，可以重写 Settings.xml 中的配置值和 ServiceManifest.xml 文件中的环境变量。 可以在导入的服务清单中设置有关终结点绑定、安全与访问权限和包共享的 **Policies**（在前面的示例中未设置）。  有关详细信息，请参阅[为应用程序配置安全策略](service-fabric-application-runas-security.md)。

**DefaultServices** 声明每当一个应用程序依据此应用程序类型进行实例化时自动创建的服务实例。 默认服务只是提供便利，创建后，其行为皆如常规服务。 它们与应用程序实例中的任何其他服务一起升级，也可将其删除。 应用程序清单可以包含多个默认服务。

**Certificates**（在前面的示例中未设置）声明用于[设置 HTTPS 终结点](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service)或[加密应用程序清单中的机密](service-fabric-application-secret-management.md)的证书。

**Policies**（在前面的示例中未设置）描述要在应用程序级别设置的日志收集、[默认运行方式帐户](service-fabric-application-runas-security.md)、[运行状况](service-fabric-health-introduction.md#health-policies)和[安全访问](service-fabric-application-runas-security.md)策略。

**Principals**（在前面的示例中未设置）描述[运行服务和安全服务资源](service-fabric-application-runas-security.md)所需的安全主体（用户或组）。  主体在 **Policies** 节中引用。

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="next-steps"></a>后续步骤
- [打包应用程序](service-fabric-package-apps.md)并准备好进行部署。
- [部署和删除应用程序](service-fabric-deploy-remove-applications.md)。
- [为不同的应用程序实例配置参数和环境变量](service-fabric-manage-multiple-environment-app-configuration.md)。
- [为应用程序配置安全策略](service-fabric-application-runas-security.md)。
- [设置 HTTPS 终结点](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service)。
- [加密应用程序清单中的机密](service-fabric-application-secret-management.md)

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!-- Update_Description: new articles service fabric application and service manifests -->