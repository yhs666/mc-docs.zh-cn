---
title: include 文件
description: include 文件
services: virtual-machines-linux
author: rockboyfor
ms.service: virtual-machines-linux
ms.topic: include
origin.date: 02/11/2019
ms.date: 10/14/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 4d0fb4e95cff66bc38bc6cc7af18d7bcc4b813ba
ms.sourcegitcommit: c9398f89b1bb6ff0051870159faf8d335afedab3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2019
ms.locfileid: "72272586"
---
## <a name="supported-distributions-and-drivers"></a>支持的分发和驱动程序

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA 驱动程序

仅下表列出的 Linux 发行版支持适用于 NCv3 系列 VM 的 NVIDIA CUDA 驱动程序。 本文发布时，CUDA 驱动程序信息为最新版本。 有关最新 CUDA 驱动程序，请访问 [NVIDIA](https://developer.nvidia.com/cuda-zone) 网站。 确保安装或升级到最新 CUDA 驱动程序分发软件包。 

<!-- Not Available on NC, NCv2, and ND-series-->
<!-- Not Available on (optional for NV-series)-->


<!-- Not Availale on  [Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md)-->

| 分发 | 驱动程序 |
| --- | -- | 
| Ubuntu 16.04 LTS、18.04 LTS<br/><br/> 基于 CentOS 的 7.3、7.4、7.5、7.6 | NVIDIA CUDA 10.1，驱动程序分支为 R418 |

<!-- Not Available on Red Hat Enterprise Linux 7.3 or 7.4-->

<!-- Not Available on ### NVIDIA GRID drivers-->

<!-- Not Available on NV-series-->
<!-- Not Available on [Red Hat Knowledgebase article](https://access.redhat.com/articles/1067)-->
<!-- Update_Description: update meta properties, wording update -->