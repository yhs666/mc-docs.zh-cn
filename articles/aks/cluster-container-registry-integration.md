---
title: 将将容器注册表与 Azure Kubernetes 服务集成
description: 了解如何将 Azure Kubernetes 服务 (AKS) 与 Azure 容器注册表 (ACR) 集成
services: container-service
author: rockboyfor
manager: digimobile
ms.service: container-service
ms.topic: article
origin.date: 08/15/2018
ms.date: 08/26/2019
ms.author: v-yeche
ms.openlocfilehash: 2f99223aef07d5ecb7d57991e6415bbdae4f993e
ms.sourcegitcommit: 599d651afb83026938d1cfe828e9679a9a0fb69f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2019
ms.locfileid: "69993781"
---
<!--Verify successfully-->

# <a name="preview---authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>预览 - 使用 Azure 容器注册表从 Azure Kubernetes 服务进行身份验证

结合使用 Azure 容器注册表 (ACR) 和 Azure Kubernetes 服务 (AKS) 时，需要建立身份验证机制。 本文详细介绍这两种 Azure 服务之间的建议身份验证配置。

可以使用 Azure CLI 通过几个简单的命令设置 AKS 与 ACR 的集成。

> [!IMPORTANT]
> AKS 预览功能是自助式选择加入功能。 预览版“按原样”提供，并且仅在“可用情况下”提供，不包含在服务级别协议和有限保障中。 AKS 预览版的内容部分包含在客户支持中，我们只能尽力提供支持。 因此，这些功能不应用于生产。 有关其他信息，请参阅以下支持文章：
>
> * [AKS 支持策略](support-policies.md)
> * [Azure 支持常见问题](faq.md)

## <a name="before-you-begin"></a>准备阶段

必须满足以下条件：

* **Azure 订阅**上的**所有者**或 **Azure 帐户管理员**角色
* 此外还需 Azure CLI 2.0.70 或更高版本，以及 aks-preview 0.4.8 扩展
* 需要在客户端上[安装 Docker](https://docs.docker.com/install/)，并且需要访问 [docker 中心](https://hub.docker.com/)

## <a name="install-latest-aks-cli-preview-extension"></a>安装最新的 AKS CLI 预览版扩展

需要 **aks-preview 0.4.8** 扩展或更高版本。

```azurecli
az extension remove --name aks-preview 
az extension add -y --name aks-preview
```

## <a name="create-a-new-aks-cluster-with-acr-integration"></a>通过 ACR 集成创建新的 AKS 群集

可以在一开始创建 AKS 群集时设置 AKS 与 ACR 的集成。  若要允许 AKS 群集与 ACR 交互，请使用 Azure Active Directory **服务主体**。 以下 CLI 命令在指定的资源组中创建 ACR，并为服务主体配置相应的 **ACRPull** 角色。 如果 *acr-name* 不存在，则会自动创建默认的 ACR 名称 `aks<resource-group>acr`。  为下面的参数提供有效值。  括号中的参数为可选。
```azurecli
az login
az aks create -n myAKSCluster -g myResourceGroup --enable-acr [--acr <acr-name-or-resource-id>]
```
此步骤可能需要几分钟才能完成。

## <a name="create-acr-integration-for-existing-aks-clusters"></a>为现有的 AKS 群集创建 ACR 集成。

为下面的 **acr-name** 和 **acr-resource-id** 提供有效值，将 ACR 与现有的 ACR 群集集成。

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --enable-acr --acr <acrName>
az aks update -n myAKSCluster -g myResourceGroup --enable-acr --acr <acr-resource-id>
```

## <a name="log-in-to-your-acr"></a>登录到 ACR

使用以下命令登录到 ACR。  将 <acrname> 参数替换为你的 ACR 名称。  例如，默认值为 **aks<your-resource-group>acr**。

```azurecli
az acr login -n <acrName>
```

## <a name="pull-an-image-from-docker-hub-and-push-to-your-acr"></a>从 Docker 中心拉取一个映像并将其推送到 ACR

从 Docker 中心拉取一个映像，对该映像进行标记，然后将其推送到 ACR。

```console
acrloginservername=$(az acr show -n <acrname> -g <myResourceGroup> --query loginServer -o tsv)
docker pull nginx
```

```
$ docker tag nginx $acrloginservername/nginx:v1
$ docker push $acrloginservername/nginx:v1

The push refers to repository [someacr1.azurecr.cn/nginx]
fe6a7a3b3f27: Pushed
d0673244f7d4: Pushed
d8a33133e477: Pushed
v1: digest: sha256:dc85890ba9763fe38b178b337d4ccc802874afe3c02e6c98c304f65b08af958f size: 948
```

## <a name="update-the-state-and-verify-pods"></a>更新状态并验证 Pod

执行以下步骤以验证部署。

```azurecli
az aks get-credentials -g myResourceGroup -n myAKSCluster
```

查看 yaml 文件，然后编辑映像属性，将值替换为你的 ACR 登录服务器、映像和标记。

```
$ cat acr-nginx.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx0-deployment
  labels:
    app: nginx0-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx0
  template:
    metadata:
      labels:
        app: nginx0
    spec:
      containers:
      - name: nginx
        image: <replace this image property with you acr login server, image and tag>
        ports:
        - containerPort: 80

$ kubectl apply -f acr-nginx.yaml
$ kubectl get pods

You should have two running pods.

NAME                                 READY   STATUS    RESTARTS   AGE
nginx0-deployment-669dfc4d4b-x74kr   1/1     Running   0          20s
nginx0-deployment-669dfc4d4b-xdpd6   1/1     Running   0          20s
```

<!-- LINKS - external -->

[AKS AKS CLI]:  https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create

<!-- Update_Description: new article about cluster container registry integration -->
<!--ms.date: 08/26/2019-->