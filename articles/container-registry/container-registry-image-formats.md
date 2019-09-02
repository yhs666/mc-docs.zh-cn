---
title: Azure 容器注册表内容格式
description: 了解 Azure 容器注册表中支持的内容格式。
services: container-registry
author: rockboyfor
manager: digimobile
ms.service: container-registry
ms.topic: article
origin.date: 04/18/2019
ms.date: 06/03/2019
ms.author: v-yeche
ms.openlocfilehash: e1dc576637e4d19fd7c890de4877606b9ba297b0
ms.sourcegitcommit: d75eeed435fda6e7a2ec956d7c7a41aae079b37c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66195380"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Azure 容器注册表中支持的内容格式

使用 Azure 容器注册表中的专用存储库来管理以下内容格式之一。 

## <a name="docker-compatible-container-images"></a>兼容 Docker 的容器映像

支持以下 Docker 容器映像格式：

* [Docker 映像清单 V2，架构 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker 映像清单 V2，架构 2](https://docs.docker.com/registry/spec/manifest-v2-2/) - 包括了允许注册表在单个“映像:标记”引用下存储多平台映像的清单列表

## <a name="oci-images"></a>OCI 映像

Azure 容器注册表还支持符合[开放容器计划 (OCI) 映像格式规范](https://github.com/opencontainers/image-spec/blob/master/spec.md)的映像。 打包格式包括[奇点映像格式 (SIF)](https://www.sylabs.io/2018/03/sif-containing-your-containers/)。

## <a name="helm-charts"></a>Helm 图表

Azure 容器注册表可以承载 [Helm 图表](https://helm.sh/)的存储库，该存储库是用来快速为 Kubernetes 管理和部署应用程序的一种打包格式。 支持 [Helm 客户端](https://docs.helm.sh/using_helm/#installing-helm)版本 2.11.0 或更高版本。

## <a name="next-steps"></a>后续步骤

* 请参阅如何使用 Azure 容器注册表[推送和拉取](container-registry-get-started-docker-cli.md)映像。

<!-- Not Available on * [ACR tasks](container-registry-tasks-overview.md)-->
* 使用 [Moby BuildKit](https://github.com/moby/buildkit) 以 OCI 格式生成并打包容器。

<!-- Not Available on * [Helm repository](container-registry-helm-repos.md)-->