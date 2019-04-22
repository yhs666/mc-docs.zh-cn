---
title: 如何升级用于容器的 Azure Monitor（预览版）代理 | Azure Docs
description: 本文介绍了如何升级用于容器的 Azure Monitor 使用的 Log Analytics 代理。
services: azure-monitor
documentationcenter: ''
author: lingliw
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/21/19
ms.author: v-lingwu
ms.openlocfilehash: 602cfa98d6d969dad5863bd008c40c4b3aae8fbc
ms.sourcegitcommit: f9d082d429c46cee3611a78682b2fc30e1220c87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2019
ms.locfileid: "59566245"
---
# <a name="how-to-upgrade-the-azure-monitor-for-containers-preview-agent"></a>如何升级用于容器的 Azure Monitor（预览版）代理
用于容器的 Azure Monitor 使用适用于 Linux 的 Log Analytics 代理的容器化版本。 发布该代理的新版本时，在 Azure Kubernetes 服务 (AKS) 上承载的托管 Kubernetes 群集上，该代理会自动升级。  

如果该代理升级失败，可参阅本文中的手动升级代理过程。 要关注发布的版本，请参阅[代理发布公告](https://github.com/microsoft/docker-provider/tree/ci_feature_prod)。   

## <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>在受监视的 Kubernetes 群集上升级代理
升级代理的过程包括两个明确的步骤。 第一个步骤是使用 Azure CLI 禁止通过用于容器的 Azure Monitor 进行监视。  请遵循[禁用监视](container-insights-optout.md?toc=%2fmonitoring%2ftoc.json#azure-cli)一文中介绍的步骤。 可以使用 Azure CLI 从群集的节点中删除代理，不会影响解决方案以及工作区中存储的相应数据。 

>[!NOTE]
>执行此维护活动时，群集中的节点不会转发所收集的数据，并且性能视图不会显示从删除代理到安装新版本这段时间内的数据。 
>

若要安装代理的新版本，请使用 Azure CLI 遵循[加入监视](container-insights-onboard.md?toc=%2fmonitoring%2ftoc.json)一文中介绍的步骤来完成此过程。  

启用监视后，可能需要约 15 分钟才能查看群集的更新后运行状况指标。 若要验证代理是否成功升级，请运行以下命令：`kubectl logs omsagent-484hw --namespace=kube-system`

状态应类似于以下示例，其中 omi 和 omsagent 的值应与[代理发布历史记录](https://github.com/microsoft/docker-provider/tree/ci_feature_prod)中指定的最新版本相匹配。  

    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.5
    omsagent 1.6.0-163
    docker-cimprov 1.0.0.31

## <a name="next-steps"></a>后续步骤
如果升级代理时遇到问题，请查看[故障排除指南](container-insights-troubleshoot.md)来获得支持。



