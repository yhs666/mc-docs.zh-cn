---
title: Azure 容器注册表中的最佳做法
description: 通过遵循这些最佳做法，了解如何有效使用 Azure 容器注册表。
services: container-registry
author: rockboyfor
manager: digimobile
ms.service: container-registry
ms.topic: quickstart
origin.date: 04/10/2018
ms.date: 07/02/2018
ms.author: v-yeche
ms.openlocfilehash: 9a951023e9c0e5e689d2b316c6ae30ca6ecbe7c4
ms.sourcegitcommit: 6d4ae5e324dbad3cec8f580276f49da4429ba1a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2018
ms.locfileid: "39167701"
---
# <a name="best-practices-for-azure-container-registry"></a>Azure 容器注册表的最佳做法

通过遵循这些最佳做法，可帮助最大化性能并在 Azure 中经济、高效地利用私有 Docker 注册表。

## <a name="network-close-deployment"></a>临近网络部署

在部署容器的 Azure 区域中创建容器注册表。 将注册表置于容器主机临近网络的区域中可帮助降低延迟和成本。

临近网络部署是使用私有容器注册表的主要原因之一。 Docker 映像具有有效的[分层构造](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/)，可实现增量部署。 但是，新节点需要拉取给定映像所需的全部构造层。 此初始 `docker pull` 可以快速增加多个千兆字节。 将私有注册表置于临近部署的位置可最小化网络延迟。
此外，所有公有云（包括 Azure）都实施了网络出口费用。 除了延迟之外，将映像从一个数据中心拉取到另一个数据中心还会增加网络出口费用。

<!-- Notice: Need to verfiy
## Geo-replicate multi-region deployments

Use Azure Container Registry's [geo-replication](container-registry-geo-replication.md) feature if you're deploying containers to multiple regions. Whether you're serving global customers from local data centers or your development team is in different locations, you can simplify registry management and minimize latency by geo-replicating your registry. Geo-replication is available only with [Premium](container-registry-skus.md) registries.
-->
若要了解如何使用异地复制，请参阅 [Azure 容器注册表中的异地复制](container-registry-tutorial-prepare-registry.md)教程，该教程分为三部分。

## <a name="repository-namespaces"></a>存储库命名空间

通过利用存储库命名空间，可以在组织中的多个组之间共享单个注册表。 可在部署和团队之间共享注册表。 Azure 容器注册表支持嵌套的命名空间，可实现组隔离。

例如，考虑以下容器映像标记。 在公司范围内使用的映像（如 `aspnetcore`）位于根命名空间中，而生产和营销组拥有的容器映像都使用其自己的命名空间。

```
contoso.azurecr.cn/aspnetcore:2.0
contoso.azurecr.cn/products/widget/web:1
contoso.azurecr.cn/products/bettermousetrap/refundapi:12.3
contoso.azurecr.cn/marketing/2017-fall/concertpromotions/campaign:218.42
```

## <a name="dedicated-resource-group"></a>专用资源组

由于容器注册表是跨多个容器主机使用的资源，因此注册表应位于其自己的资源组中。

虽然可以试用特定的主机类型（如 Azure 容器实例），但完成操作后可能会删除容器实例。 但是，你可能还想保留推送到 Azure 容器注册表的映像集合。 通过将注册表置于其自己的资源组中，可以最小化删除容器实例资源组时在注册表中意外删除映像集合的风险。

## <a name="authentication"></a>身份验证

Azure 容器注册表的身份验证有两种主要方案：单个身份验证和服务（或“无外设”）身份验证。 下表提供了这两个方案的简要概述，以及每个方案的推荐身份验证方法。

| 类型 | 示例方案 | 推荐的方法 |
|---|---|---|
| 单个标识 | 开发者从/向其开发计算机推送映像。 | [az acr login](https://docs.azure.cn/zh-cn/cli/acr?view=azure-cli-latest#az-acr-login) |
| 无外设/服务标识 | 用户未直接参与的生成和部署管道。 | [服务主体](container-registry-authentication.md#service-principal) |

有关 Azure 容器注册表身份验证的详细信息，请参阅 [Azure 容器注册表的身份验证](container-registry-authentication.md)。

## <a name="manage-registry-size"></a>管理注册表大小

每个[容器注册表 SKU][container-registry-skus] 的存储约束旨在与典型方案保持一致，即基本 SKU 适用于入门，标准 SKU 适用于大部分生产应用程序，高级 SKU 适用于超大规模提升性能和[异地复制][container-registry-geo-replication]。 在注册表的整个生命周期中，应定期删除未使用的内容，管理注册表大小。

在 Azure 门户的容器注册表“概述”中，可以查看注册表的当前使用情况：

![Azure 门户中的注册表使用情况信息][registry-overview-quotas]

可以使用 [Azure CLI][azure-cli] 或 [Azure 门户][azure-portal]管理注册表大小。 仅托管 SKU（基本、标准、高级）支持删除存储库和映像--不能在经典注册表中删除存储库、映像或标记。

### <a name="delete-in-azure-cli"></a>在 Azure CLI 中删除

使用 [az acr repository delete][az-acr-repository-delete] 命令可删除存储库或其中的内容。

若要删除存储库（包括存储库中的所有标记和映像层数据），请在执行 [az acr repository delete][az-acr-repository-delete] 命令时仅指定存储库名称。 在以下示例中，我们删除 *myapplication* 存储库以及该存储库中的所有标记和映像层数据。

```azurecli
az acr repository delete --name myregistry --repository myapplication
```

也可使用 `--tag` 和 `--manifest` 参数删除存储库中的映像数据。 有关这些参数的详细信息，请参阅 [az acr repository delete 命令参考][az-acr-repository-delete]。

### <a name="delete-in-azure-portal"></a>在 Azure 门户中进行删除

若要在 Azure 门户中删除注册表中的存储库，请首先导航到容器注册表。 然后在“服务”下选择“存储库”，右键单击要删除的存储库。 选择“删除”即可删除该存储库以及其所包含的 Docker 映像。

![在 Azure 门户中删除存储库][delete-repository-portal]

也可使用类似的方式删除存储库中的标记。 导航到存储库，在“标记”下右键单击要删除的标记，然后选择“删除”。

## <a name="next-steps"></a>后续步骤

Azure 容器注册表可用于多层（称为 SKU），每层提供不同功能。 有关可用 SKU 的详细信息，请参阅 [Azure 容器注册表 SKU](container-registry-skus.md)。

<!-- IMAGES -->
[delete-repository-portal]: ./media/container-registry-best-practices/delete-repository-portal.png
[registry-overview-quotas]: ./media/container-registry-best-practices/registry-overview-quotas.png

<!-- LINKS - Internal -->
[az-acr-repository-delete]: https://docs.azure.cn/zh-cn/cli/acr/repository?view=azure-cli-latest#az_acr_repository_delete
[azure-cli]: https://docs.azure.cn/zh-cn/cli/azure?view=azure-cli-latest
[azure-portal]: https://portal.azure.cn
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-skus]: container-registry-skus.md
<!-- Update_Description: new articles on container registry best practices -->
<!--ms.date: 07/02/2018-->