---
title: Azure 容器注册表内容格式
description: 了解 Azure 容器注册表中支持的内容格式。
services: container-registry
author: rockboyfor
manager: digimobile
ms.service: container-registry
ms.topic: article
origin.date: 10/31/2018
ms.date: 12/17/2018
ms.author: v-yeche
ms.openlocfilehash: c9770e3c3ff394e65dbb2097178039c9b1fa454e
ms.sourcegitcommit: 1db6f261786b4f0364f1bfd51fd2db859d0fc224
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2018
ms.locfileid: "53286794"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Azure 容器注册表中支持的内容格式

使用 Azure 容器注册表中的专用存储库来管理以下内容格式之一。 

## <a name="docker-compatible-container-images"></a>兼容 Docker 的容器映像

* [Docker 映像清单 V2，架构 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker 映像清单 V2，架构 2](https://docs.docker.com/registry/spec/manifest-v2-2/) - 包括了允许注册表在单个“映像:标记”引用下存储多平台映像的清单列表

* [开放容器计划 (OCI) 映像格式规范](https://github.com/opencontainers/image-spec/blob/master/spec.md) 

<!-- Not Available on ## Helm charts-->
## <a name="next-steps"></a>后续步骤

* 请参阅如何使用 Azure 容器注册表[推送和拉取](container-registry-get-started-docker-cli.md)映像。

<!-- Not Available on * [ACR tasks](container-registry-tasks-overview.md)-->
* 使用 [Moby BuildKit](https://github.com/moby/buildkit) 以 OCI 格式生成并打包容器。

<!-- Not Available on * [Helm repository](container-registry-helm-repos.md)-->