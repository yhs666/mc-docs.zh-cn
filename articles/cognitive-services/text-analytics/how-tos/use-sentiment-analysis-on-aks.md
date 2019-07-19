---
title: 运行 Azure Kubernetes 服务
titleSuffix: Text Analytics - Azure Cognitive Services
description: 将包含情绪分析映像的文本分析容器部署到 Azure Kubernetes 服务，并在 Web 浏览器中对其进行测试。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
origin.date: 06/21/2019
ms.date: 07/09/2019
ms.author: v-junlch
ms.openlocfilehash: 2d14a954369ac6576bcda94c39b1daef135ac932
ms.sourcegitcommit: 8f49da0084910bc97e4590fc1a8fe48dd4028e34
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "67845000"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-kubernetes-services-aks"></a>将情绪分析容器部署到 Azure Kubernetes 服务 (AKS)

了解如何将包含情绪分析映像的认知服务[文本分析](/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers)容器部署到 Azure Kubernetes 服务 (AKS)。 该过程演示了创建文本分析资源的方法、创建关联的情绪分析映像的方法，以及在浏览器中练习前两项的相关业务流程的功能。 使用容器可以将开发人员的注意力从管理基础结构转移到应用程序开发上。

## <a name="prerequisites"></a>先决条件

此过程要求必须在本地安装和运行多个工具。 

* 使用 Azure 订阅。 如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。
* 文本编辑器，例如：[Visual Studio Code](https://code.visualstudio.com/download)。
* 安装 [Azure CLI](/cli/install-azure-cli?view=azure-cli-latest)。
* 安装 [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/)。
* 具有适当定价层的 Azure 资源。 并非所有定价层都适用于此容器：
    * 仅具有 F0 或标准定价层的文本分析资源  。
    * 具有 S0 定价层的认知服务资源  。

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

## <a name="create-an-azure-kubernetes-service-aks-cluster-resource"></a>创建 Azure Kubernetes 服务 (AKS) 群集资源

1. 转到与 Kubernetes 服务相对应的[“创建”](https://portal.azure.cn/#create/microsoft.aks)。

1. 在“基本信息”选项卡中输入以下详细信息： 

    |设置|Value|
    |--|--|
    |订阅|选择相应的订阅|
    |资源组|选择可用的资源组|
    |Kubernetes 群集名称|所需名称（小写）|
    |区域|选择附近的位置|
    |Kubernetes 版本|1.12.8（默认）|
    |DNS 名称前缀|自动创建，但可以选择重写|
    |节点大小|标准 DS2 v2：<br>`2 vCPUs`, `7 GB`|
    |节点计数|将滑块留在默认值的位置|

1. 在“身份验证”选项卡上，让“服务主体”和“启用 RBAC”保留默认值。   
1. 在“网络”选项卡上，输入以下选择： 

    |设置|Value|
    |--|--|
    |网络配置|基本|

1. 在“监视”选项卡上，确保将“启用容器监视”设置为“是”，并让“Log Analytics 工作区”保留默认值    
1. 在“标记”选项卡上，暂时让名称/值对保留空白  。
1. 单击“查看并创建”  。
1. 验证通过后，单击“创建”。 

> [!NOTE]
> 如果验证失败，可能是由于服务主体错误。  导航回“身份验证”选项卡，然后回到“查看 + 创建”，应该会在其中执行验证，然后验证会通过。  

## <a name="deploy-text-analytics-container-to-an-aks-cluster"></a>将文本分析容器部署到 AKS 群集

1. 打开 Azure CLI 并登录到 Azure

    ```azurecli
    az login
    ```

1. 登录到 AKS 群集（将 `your-cluster-name` 和 `your-resource-group` 替换为相应的值）

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    此命令在执行后会报告类似以下内容的消息：

    ```console
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > 如果在 Azure 帐户上有多个可用订阅，而 `az aks get-credentials` 命令返回错误，则表明你使用了错误的订阅，这是一个常见问题。 直接将 Azure CLI 会话的上下文设置为使用你在创建这些资源时使用的订阅，然后重试。
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. 打开所选文本编辑器（此示例使用 __Visual Studio Code__）：

    ```azurecli
    code .
    ```

1. 在文本编辑器中创建名为 _sentiment.yaml_ 的新文件，然后将以下 YAML 粘贴到其中。 确保将 `billing/value` 和 `apikey/value` 替换为自己的值。

    ```yaml
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: sentiment
    spec:
      template:
        metadata:
          labels:
            app: sentiment-app
        spec:
          containers:
          - name: sentiment
            image: mcr.microsoft.com/azure-cognitive-services/sentiment
            ports:
            - containerPort: 5000
            env:
            - name: EULA
              value: "accept"
            - name: billing
              value: # < Your endpoint >
            - name: apikey
              value: # < Your API Key >
     
    --- 
    apiVersion: v1
    kind: Service
    metadata:
      name: sentiment
    spec:
      type: LoadBalancer
      ports:
      - port: 5000
      selector:
        app: sentiment-app
    ```

1. 保存文件并关闭文本编辑器。
1. 执行 Kubernetes 的 `apply` 命令，将 _sentiment.yaml_ 作为其目标：

    ```console
    kubectl apply -f sentiment.yaml
    ```

    当命令成功应用部署配置以后，会出现类似于以下输出的消息：

    ```
    deployment.apps "sentiment" created
    service "sentiment" created
    ```
1. 验证 POD 是否已部署：

    ```console
    kubectl get pods
    ```

    此时会输出 POD 的运行状态：

    ```
    NAME                         READY     STATUS    RESTARTS   AGE
    sentiment-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. 验证服务是否可用，然后获取 IP 地址：

    ```console
    kubectl get services
    ```

    此时会输出  POD 中情绪服务的运行状态：

    ```
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    sentiment    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

 