---
title: 在 Kubernetes 和 Helm 中使用语音服务容器
titleSuffix: Azure Cognitive Services
description: 我们将使用 Kubernetes 和 Helm 定义文本转语音容器映像，以此创建一个 Kubernetes 包。 此包将部署到本地 Kubernetes 群集。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin-date: 08/26/2019
ms.date: 09/23/2019
ms.author: v-tawe
ms.openlocfilehash: 86dae764b736827adb35d46cb612872ee676a38f
ms.sourcegitcommit: aea45739ba114a6b069f782074a70e5dded8a490
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72275552"
---
# <a name="use-speech-service-container-with-kubernetes-and-helm"></a>在 Kubernetes 和 Helm 中使用语音服务容器

在本地管理语音容器的一种做法是使用 Kubernetes 和 Helm。 我们将使用 Kubernetes 和 Helm 定义文本转语音容器映像，以此创建一个 Kubernetes 包。 此包将部署到本地 Kubernetes 群集。 最后，我们将了解如何测试已部署的服务和各种配置选项。 有关在不使用 Kubernetes 业务流程的情况下运行 Docker 容器的详细信息，请参阅[安装和运行语音服务容器](speech-container-howto.md)。

## <a name="prerequisites"></a>先决条件

在本地使用语音容器之前，必须满足以下先决条件：

|必须|目的|
|--|--|
| Azure 帐户 | 如果没有 Azure 订阅，可在开始前创建一个[试用帐户][trial-azure-account]。 |
| 容器注册表访问权限 | Kubernetes 需要有权访问容器注册表才能将 Docker 映像提取到群集中。 首先需要[请求对容器注册表的访问权限][speech-preview-access]。 |
| Kubernetes CLI | 需要使用 [Kubernetes CLI][kubernetes-cli] 来管理容器注册表中的共享凭据。 在安装 Helm（Kubernetes 包管理器）之前，也需要有 Kubernetes。 |
| Helm CLI | 在安装 [Helm CLI][helm-install] 的过程中，还需要初始化 Helm，这会安装 [Tiller][tiller-install]。 |
|语音资源 |若要使用这些容器，必须具有：<br><br>“语音”Azure 资源，用于获取关联的计费密钥和计费终结点 URI。  这两个值可以从 Azure 门户中的“语音概述”和“密钥”页面获得，并且是启动容器时所必需的。 <br><br>**{API_KEY}** ：资源密钥<br><br>**{ENDPOINT_URI}** ：终结点 URI 示例：`https://chinaeast.api.cognitive.chinacloudapi.cn/sts/v1.0`|

## <a name="the-recommended-host-computer-configuration"></a>建议的主机配置

有关详细信息，请参考[语音服务容器主机][speech-container-host-computer]。 此 Helm 图表根据用户指定的解码（并发请求）数自动计算 CPU 和内存要求。  此外，它还会根据音频/文本输入优化是否配置为 `enabled` 来进行调整。 默认情况下，Helm 图表假设指定了两个并发请求并禁用了优化。

| 服务 | CPU/容器 | 内存/容器 |
|--|--|--|
| **文本转语音** | 一个并发请求至少需要 500 个毫核心。 如果已启用 `optimizeForTurboMode`，则需要 1,000 个毫核心。 （默认值：两个并发请求） | 必需：1 GB<br> 限制：2 GB |

## <a name="connect-to-the-kubernetes-cluster"></a>连接到 Kubernetes 群集

主机预期有一个可用的 Kubernetes 群集。 请参阅这篇有关[部署 Kubernetes 群集](../../aks/tutorial-kubernetes-deploy-cluster.md)的教程，对如何将 Kubernetes 群集部署到主机有一个概念性的了解。

### <a name="sharing-docker-credentials-with-the-kubernetes-cluster"></a>与 Kubernetes 群集共享 Docker 凭据

若要允许 Kubernetes 群集对 `containerpreview.azurecr.cn` 容器注册表中配置的映像执行 `docker pull`，需将 Docker 凭据传输到群集中。 执行以下 [`kubectl create`][kubectl-create] 命令，基于“容器注册表访问权限”先决条件提供的凭据创建 Docker 注册表机密。 

在所选的命令行接口中运行以下命令。 请务必将 `<username>`、`<password>` 和 `<email-address>` 替换为容器注册表凭据。

```console
kubectl create secret docker-registry containerpreview \
    --docker-server=containerpreview.azurecr.cn \
    --docker-username=<username> \
    --docker-password=<password> \
    --docker-email=<email-address>
```

> [!NOTE]
> 如果你已有权访问 `containerpreview.azurecr.cn` 容器注册表，则可以改用常规标志创建 Kubernetes 机密。 考虑针对 Docker 配置 JSON 执行以下命令。
> ```console
>  kubectl create secret generic containerpreview \
>      --from-file=.dockerconfigjson=~/.docker/config.json \
>      --type=kubernetes.io/dockerconfigjson
> ```

成功创建机密后，控制台中会列显以下输出。

```console
secret "containerpreview" created
```

若要验证是否已创建机密，请结合 `secrets` 标志执行 [`kubectl get`][kubectl-get]。

```console
kuberctl get secrets
```

执行 `kubectl get secrets` 会列显所有已配置的机密。

```console
NAME                  TYPE                                  DATA      AGE
containerpreview      kubernetes.io/dockerconfigjson        1         30s
```

## <a name="configure-helm-chart-values-for-deployment"></a>配置用于部署的 Helm 图表值

访问 [Microsoft Helm Hub][ms-helm-hub]，查看 Microsoft 提供的所有已公开的 Helm 图表。 从 Microsoft Helm Hub 中，可以看到“认知服务语音本地图表”。  “认知服务语音本地图表”是我们要安装的图表，但我们必须先创建一个包含显式配置的 `config-values.yaml` 文件。  首先，将 Microsoft 存储库添加到 Helm 实例。

```console
helm repo add microsoft https://microsoft.github.io/charts/repo
```

接下来，配置 Helm 图表值。 将以下 YAML 复制并粘贴到名为 `config-values.yaml` 的文件。 有关自定义“认知服务语音本地 Helm 图表”的详细信息，请参阅[自定义 Helm 图表](#customize-helm-charts)。  请将 `# {ENDPOINT_URI}` 和 `# {API_KEY}` 注释替换为自己的值。

<!-- speech-to-text -->

```yaml
# These settings are deployment specific and users can provide customizations

# text-to-speech configurations
textToSpeech:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForTurboMode: true
  image:
    registry: containerpreview.azurecr.cn
    repository: microsoft/cognitive-services-text-to-speech
    tag: latest
    pullSecrets:
      - containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # {ENDPOINT_URI}
      apikey: # {API_KEY}
```

> [!IMPORTANT]
> 如果未提供 `billing` 和 `apikey` 值，服务将在 15 分钟后过期。 同样，由于服务不可用，验证将会失败。

### <a name="the-kubernetes-package-helm-chart"></a>Kubernetes 包（Helm 图表）

Helm 图表包含要从 `containerpreview.azurecr.cn` 容器注册表提取的 Docker 映像的配置。 

> [Helm 图表][helm-charts]是描述一组相关 Kubernetes 资源的文件集合。 单个图表既可用于部署简单的资源（例如 Memcached Pod），也可用于部署复杂的资源（例如，包含 HTTP 服务器、数据库、缓存等的完整 Web 应用堆栈）。

提供的 Helm 图表将从 `containerpreview.azurecr.cn` 容器注册表中提取语音服务（文本转语音服务）的 Docker 映像。 

## <a name="install-the-helm-chart-on-the-kubernetes-cluster"></a>在 Kubernetes 群集上安装 Helm 图表

若要安装 Helm 图表，需要执行 [`helm install`][helm-install-cmd] 命令（请将 `<config-values.yaml>` 替换为相应的路径和文件名参数）。  下面提到的 [ Helm 图表已在`microsoft/cognitive-services-speech-onpremise`此处的 Microsoft Helm Hub][ms-helm-hub-speech-chart] 上提供。

```console
helm install microsoft/cognitive-services-speech-onpremise \
    --version 0.1.1 \
    --values <config-values.yaml> \
    --name onprem-speech
```

下面是成功执行安装命令后预期会看到的示例输出：

```console
NAME:   onprem-speech
LAST DEPLOYED: Tue Jul  2 12:51:42 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                             READY  STATUS             RESTARTS  AGE
text-to-speech-56f8fb685b-4jtzh  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-frwxf  0/1    Pending            0         0s

==> v1/Service
NAME            TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)       AGE
text-to-speech  LoadBalancer  10.0.125.187  <pending>    80:31247/TCP  0s

==> v1beta1/PodDisruptionBudget
NAME                                MIN AVAILABLE  MAX UNAVAILABLE  ALLOWED DISRUPTIONS  AGE
text-to-speech-poddisruptionbudget  N/A            20%              0                    1s

==> v1beta2/Deployment
NAME            READY  UP-TO-DATE  AVAILABLE  AGE
text-to-speech  0/2    2           0          0s

==> v2beta2/HorizontalPodAutoscaler
NAME                       REFERENCE                  TARGETS        MINPODS  MAXPODS  REPLICAS  AGE
text-to-speech-autoscaler  Deployment/text-to-speech  <unknown>/50%  2        10       0         0s


NOTES:
cognitive-services-speech-onpremise has been installed!
Release is named onprem-speech
```

Kubernetes 部署可能需要几分钟才能完成。 若要确认 Pod 和服务是否已正确部署且可用，请执行以下命令：

```console
kubectl get all
```

预期会看到类似于以下输出的内容：

```console
NAME                                  READY     STATUS    RESTARTS   AGE
pod/text-to-speech-56f8fb685b-4jtzh   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-frwxf   1/1       Running   0          34m

NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
service/kubernetes       ClusterIP      10.0.0.1       <none>           443/TCP        3h
service/text-to-speech   LoadBalancer   10.0.125.187   65.52.233.162    80:31247/TCP   34m

NAME                             DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/text-to-speech   2         2         2            2           34m

NAME                                        DESIRED   CURRENT   READY     AGE
replicaset.apps/text-to-speech-56f8fb685b   2         2         2         34m

NAME                                                            REFERENCE                   TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/text-to-speech-autoscaler   Deployment/text-to-speech   0%/50%    2         10        2          34m
```

### <a name="verify-helm-deployment-with-helm-tests"></a>通过 Helm 测试验证 Helm 部署

安装的 Helm 图表定义了 Helm 测试以方便验证。  这些测试将验证服务就绪性。 为了验证**文本转语音**服务，我们将执行 [Helm test][helm-test] 命令。

```console
helm test onprem-speech
```

> [!IMPORTANT]
> 如果 POD 状态不是 `Running` 或 `AVAILABLE` 列下未列出部署，则这些测试将会失败。 请耐心等待，此测试可能需要 10 分钟以上才能完成。

这些测试将输出各种状态结果：

```console
RUNNING: text-to-speech-readiness-test
PASSED: text-to-speech-readiness-test
```

如果不执行 Helm 测试，也可以通过 `kubectl get all` 命令收集外部 IP 地址和相应的端口。   使用该 IP 和端口打开 Web 浏览器，并导航到 `http://<external-ip>:<port>:/swagger/index.html` 以查看 API Swagger 页。

## <a name="customize-helm-charts"></a>自定义 Helm 图表

Helm 图表是分层的。 分层结构可以实现图表继承，同时还与明确性的理念相符，使更具体的设置替代继承的规则。

[!INCLUDE [Speech umbrella-helm-chart-config](includes/speech-umbrella-helm-chart-config.md)]

[!INCLUDE [Text-to-Speech Helm Chart Config](includes/text-to-speech-chart-config.md)]

## <a name="next-steps"></a>后续步骤

有关在 Azure Kubernetes 服务 (AKS) 中使用 Helm 安装应用程序的更多详细信息，请[访问此页][installing-helm-apps-in-aks]。

> [!div class="nextstepaction"]
> [认知服务容器][cog-svcs-containers]

<!-- LINKS - external -->
[trial-azure-account]: https://www.azure.cn/pricing/1rmb-trial-full/?form-type=identityauth
[git-download]: https://git-scm.com/downloads
[azure-cli]: https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest
[docker-engine]: https://www.docker.com/products/docker-engine
[kubernetes-cli]: https://kubernetes.io/docs/tasks/tools/install-kubectl
[helm-install]: https://helm.sh/docs/using_helm/#installing-helm
[helm-install-cmd]: https://helm.sh/docs/helm/#helm-install
[tiller-install]: https://helm.sh/docs/install/#installing-tiller
[helm-charts]: https://helm.sh/docs/developing_charts
[speech-preview-access]: https://aka.ms/speechcontainerspreview
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-test]: https://helm.sh/docs/helm/#helm-test
[ms-helm-hub]: https://hub.helm.sh/charts/microsoft
[ms-helm-hub-speech-chart]: https://hub.helm.sh/charts/microsoft/cognitive-services-speech-onpremise

<!-- LINKS - internal -->
[speech-container-host-computer]: speech-container-howto.md#the-host-computer
[installing-helm-apps-in-aks]: ../../aks/kubernetes-helm.md
[cog-svcs-containers]: ../cognitive-services-container-support.md
