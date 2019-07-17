---
title: 配置用于容器的 Azure Monitor 的代理数据收集 | Microsoft Docs
description: 本主题介绍如何配置用于容器的 Azure Monitor 代理，以控制 stdout/stderr 和环境变量日志收集。
services: azure-monitor
documentationcenter: ''
author: lingliw
manager: digimobile
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/02/19
ms.author: v-lingwu
ms.openlocfilehash: 55d9515176b9da5db0fdbe16491f949c29ab6fc5
ms.sourcegitcommit: fd927ef42e8e7c5829d7c73dc9864e26f2a11aaa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/04/2019
ms.locfileid: "67562985"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>配置用于容器的 Azure Monitor 的代理数据收集

用于容器的 Azure Monitor 通过容器化的代理，从托管在 Azure Kubernetes 服务 (AKS) 上的托管 Kubernetes 群集中部署的容器工作负荷收集 stdout、stderr 和环境变量。 可以创建一个自定义的 Kubernetes ConfigMap 用于控制此体验，以配置代理数据收集设置。 本文演示如何根据要求创建 ConfigMap 和配置数据收集。

## <a name="configure-your-cluster-with-custom-data-collection-settings"></a>使用自定义数据收集设置配置群集

我们已提供一个模板 ConfigMap 文件，你可以使用自定义内容轻松编辑此文件，而无需从头开始创建。 在开始之前，请查看有关 [ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) 的 Kubernetes 文档，并熟悉如何创建、配置和部署 ConfigMap。 这样，就可以按命名空间或者在整个群集中筛选 stderr 和 stdout，以及筛选在所有群集 pod/节点中运行的任何容器的环境变量。

>[!IMPORTANT]
>此功能支持的最低代理版本是 microsoft/oms:ciprod06142019 或更高版本。 

### <a name="overview-of-configurable-data-collection-settings"></a>可配置的数据收集设置概述

下面是可以配置的用于控制数据收集的设置。

|键 |数据类型 |Value |说明 |
|----|----------|------|------------|
|`schema-version` |字符串（区分大小写） |v1 |这是代理在分析 ConfigMap 时使用的架构版本。 当前支持的架构版本为 v1。 不支持修改此值，评估 ConfigMap 时会拒绝修改的值。|
|`config-version` |String | | 支持在源代码管理系统/存储库中跟踪此配置文件的版本。 允许的最大字符数为 10，所有其他字符将会截掉。 |
|`[log_collection_settings.stdout] enabled =` |布尔 | true 或 false | 此设置控制是否启用 stdout 容器日志收集。 如果设置为 `true` 且未在 stdout 日志收集中排除任何命名空间（下面的 `log_collection_settings.stdout.exclude_namespaces` 设置），则会从所有群集 pod/节点中的所有容器收集 stdout 日志。 如果未在 ConfigMap 中指定，默认值为 `enabled = true`。 |
|`[log_collection_settings.stdout] exclude_namespaces =`|String | 逗号分隔的数组 |不收集其 stdout 日志的 Kubernetes 命名空间数组。 仅当 `log_collection_settings.stdout.enabled` 设置为 `true` 时，此设置才会生效。 如果未在 ConfigMap 中指定，默认值为 `exclude_namespaces = ["kube-system"]`。|
|`[log_collection_settings.stderr] enabled =` |布尔 | true 或 false |此设置控制是否启用 stderr 容器日志收集。 如果设置为 `true` 且未在 stdout 日志收集中排除任何命名空间（`log_collection_settings.stderr.exclude_namespaces` 设置），则会从所有群集 pod/节点中的所有容器收集 stderr 日志。 如果未在 ConfigMap 中指定，默认值为 `enabled = true`。 |
|`[log_collection_settings.stderr] exclude_namespaces =` |String |逗号分隔的数组 |不收集其 stderr 日志的 Kubernetes 命名空间数组。 仅当 `log_collection_settings.stdout.enabled` 设置为 `true` 时，此设置才会生效。 如果未在 ConfigMap 中指定，默认值为 `exclude_namespaces = ["kube-system"]`。 |
| `[log_collection_settings.env_var] enabled =` |布尔 | true 或 false | 此设置控制是否启用环境变量收集。 如果设置为 `false`，则不会收集所有群集 pod/节点中运行的任何容器的环境变量。 如果未在 ConfigMap 中指定，默认值为 `enabled = true`。 |

### <a name="configure-and-deploy-configmaps"></a>使用 ConfigMap 进行配置和部署

执行以下步骤，以配置 ConfigMap 配置文件并将其部署到群集。

1. [下载](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml)模板 ConfigMap yaml 文件，并将其保存为 container-azm-ms-agentconfig.yaml。  
1. 使用自定义内容编辑 ConfigMap yaml 文件。 

    - 若要排除特定命名空间的 stdout 日志收集，可以参考以下示例配置键/值：`[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`。
    - 若要禁用特定容器的环境变量收集，请设置键/值 `[log_collection_settings.env_var] enabled = true` 以全局启用变量收集，然后遵循[此处](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container)所述的步骤完成特定容器的配置。
    - 若要在群集范围禁用 stderr 日志收集，请参考以下示例配置键/值：`[log_collection_settings.stderr] enabled = false`。

1. 运行以下 kubectl 命令创建 ConfigMap：`kubectl apply -f <configmap_yaml_file.yaml>`。
    
    示例：`kubectl apply -f container-azm-ms-agentconfig.yaml`。 
    
    配置更改可能需要几分钟时间才能完成并生效，群集中的所有 omsagent pod 将会重启。 所有 omsagent pod 的重启是轮流式的重启，而不是一次性全部重启。 重启完成后，系统会显示包含结果的消息，如下所示：`configmap "container-azm-ms-agentconfig" created`。

若要验证是否已成功应用配置，请使用以下命令查看代理 pod 的日志：`kubectl logs omsagent-fdf58 -n=kube-system`。 如果 osmagent pod 存在配置错误，输出中会显示如下所示的错误：

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

错误阻止了 omsagent 分析文件，导致其重启并使用默认配置。 更正 ConfigMap 中的错误后，保存 yaml 文件，并运行以下命令应用已更新的 ConfigMap：`kubectl apply -f <configmap_yaml_file.yaml`。

## <a name="applying-updated-configmap"></a>应用已更新的 ConfigMap

如果你已经为群集部署了 ConfigMap 但想要使用较新的配置更新 ConfigMap，只需编辑以前用过的 ConfigMap 文件，然后使用前面提到的相同命令 `kubectl apply -f <configmap_yaml_file.yaml` 应用该文件。

配置更改可能需要几分钟时间才能完成并生效，群集中的所有 omsagent pod 将会重启。 所有 omsagent pod 的重启是轮流式的重启，而不是一次性全部重启。 重启完成后，系统会显示包含结果的消息，如下所示：`configmap "container-azm-ms-agentconfig" updated`。

## <a name="verifying-schema-version"></a>验证架构版本

omsagent pod 上以 pod 注释 (schema-versions) 的形式提供了支持的配置架构版本。 可以使用以下 kubectl 命令查看版本：`kubectl describe pod omsagent-fdf58 -n=kube-system`

输出中会显示如下所示的包含注释架构版本的内容：

```
    Name:           omsagent-fdf58
    Namespace:      kube-system
    Node:           aks-agentpool-95673144-0/10.240.0.4
    Start Time:     Mon, 10 Jun 2019 15:01:03 -0700
    Labels:         controller-revision-hash=589cc7785d
                    dsName=omsagent-ds
                    pod-template-generation=1
    Annotations:    agentVersion=1.10.0.1
                  dockerProviderVersion=5.0.0-0
                    schema-versions=v1 
```

## <a name="next-steps"></a>后续步骤

- 若要继续学习如何使用 Azure Monitor 以及如何监视 AKS 群集的其他方面，请参阅[查看 Azure Kubernetes 服务运行状况](container-insights-analyze.md)。
- 请参阅[日志查询示例](container-insights-log-search.md#search-logs-to-analyze-data)，以查看预定义的查询，以及用于发警报、可视化或分析群集的评估或自定义示例。