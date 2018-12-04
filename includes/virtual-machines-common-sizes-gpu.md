---
title: include 文件
description: include 文件
services: virtual-machines-windows, virtual-machines-linux
author: rockboyfor
ms.service: multiple
ms.topic: include
origin.date: 11/14/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: f772021e311975a1a437104dbdbe83821281624b
ms.sourcegitcommit: 59db70ef3ed61538666fd1071dcf8d03864f10a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676341"
---
GPU 优化 VM 大小是具有单个或多个 NVIDIA GPU 的专用虚拟机。 这些大小是针对计算密集型、图形密集型和可视化工作负荷设计的。 本文介绍有关 GPU、vCPU、数据磁盘和 NIC 的数量和类型的信息。 此分组中的每个大小还包括存储吞吐量及网络带宽。 

* ** NCv3** 大小针对计算密集型和网络密集型应用程序和算法进行了优化。 一些示例包括基于 CUDA 和 OpenCL 的应用程序和模拟、AI 和深度学习。 NCv3 系列专用于采用 NVIDIA Tesla V100 GPU 的高性能计算工作负载。

<!-- Not Available on NC, NCv2 and ND   -->
<!-- Not Available on NV and NVv2   -->

<!-- Not Available on ## NC-series-->
<!-- Not Available on ## NCv2-series-->
## <a name="ncv3-series"></a>NCv3 系列

高级存储：支持

高级存储缓存：支持

NCv3 系列 VM 采用 [NVIDIA Tesla V100](http://www.nvidia.com/content/PDF/Volta-Datasheet.pdf) GPU。 这些 GPU 可提供 NCv2 系列的 1.5 倍计算性能。 客户可将这些更新的 GPU 用于传统的 HPC 工作负荷，例如油藏模拟、DNA 测序、蛋白质分析、Monte Carlo 模拟和其他工作负荷。 NC24rs v3 配置提供了针对紧密耦合的并行计算工作负荷优化的低延迟、高吞吐量网络接口。

> [!IMPORTANT]
> 对于此大小系列，订阅中的 vCPU（核心）配额在每个区域中最初都设置为 0。 可以请求在某个[可用区域](https://www.azure.cn/zh-cn/home/features/products-by-region)中[提高此系列的 vCPU 配额](https://support.windowsazure.cn/support/support-azure)。
>

<!--Notice: URL redirect ../articles/azure-supportability/resource-manager-core-quotas-request.md to https://support.windowsazure.cn/support/support-azure -->

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | GPU | GPU 内存：GiB | 最大数据磁盘数 | 最大 NIC 数 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 |6 |112 | 736 | 1 | 16 | 12 | 4 |
| Standard_NC12s_v3 |12 |224 | 1474 | 2 | 32 | 24 | 8 |
| Standard_NC24s_v3 |24 |448 | 2948 | 4 | 64 | 32 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 8 |

1 GPU = 一个 V100 卡。

*支持 RDMA

<!-- Not Available on ## ND-series-->
<!-- Not Available on ## NV-series -->
<!-- Not Avaiable on ## NVv2-series (Preview)-->
<!-- Update_Description: update meta properties, wording update -->