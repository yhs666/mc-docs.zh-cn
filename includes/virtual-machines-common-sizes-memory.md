---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/09/2018
ms.date: 04/16/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: a3cd1b459a46b08e1bee50080f364d9d4bf20432
ms.sourcegitcommit: 6e80951b96588cab32eaff723fe9f240ba25206e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2018
---
内存优化 VM 大小提供适用于关系数据库服务器、中到大型规模的缓存和内存中分析的高内存 CPU 比率。 本文介绍了此分组中各个大小的 vCPU 数、数据磁盘数、NIC 数、存储吞吐量及网络带宽的相关信息。 
<!-- Not Available M-Series -->
<!-- Not Available on G, GS series -->
* Dv2 系列、D 系列以及对应的 DS 系列是请求更快速的 vCPU、更好的临时存储性能，或有更高内存要求之应用程序的最佳选择。  它们为许多企业级应用程序提供强大的组合。

* D 系列 VM 旨在运行需要更高计算能力和临时磁盘性能的应用程序。 D 系列 VM 为临时存储提供更快的处理器、更高的内存 vCPU 比和固态硬盘 (SSD)。 有关详细信息，请参阅 Azure 博客[新的 D 系列虚拟机大小](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)上的公告。

* Dv2 系列是原 D 系列的后续系列，其特点是 CPU 功能更强大。 Dv2 系列 CPU 比 D 系列 CPU 快大约 35%。 该系列基于最新一代的 2.4 GHz Intel Xeon® E5-2673 v3 (Haswell) 处理器，通过 Intel Turbo Boost Technology 2.0 可以达到 3.1 GHz。 Dv2 系列的内存和磁盘配置与 D 系列相同。


<!--PENDIND ON Esv3-series, Updte carefully -->

## <a name="esv3-series-sup1sup"></a>Esv3 系列 <sup>1</sup>

ACU：160-190

ESv3 系列实例基于 2.3 GHz Intel XEON® E5-2673 v4 (Broadwell) 处理器，可通过 Intel Turbo Boost Technology 2.0 达到 3.5 GHz，并使用高级存储。 Ev3 系列实例适用于内存密集型企业应用程序。

| 大小             | vCPU | 内存：GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 缓存和临时存储的最大吞吐量：IOPS/MBps（以 GiB 为单位的缓存大小） | 非缓存磁盘最大吞吐量：IOPS / MBps | 最大 NIC 数/预期网络带宽 (MBps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3 | 2      | 16          | 32             | 4              | 4,000 / 32 (50)                                                       | 3,200 / 48                                | 2 / 1,000                                   |
| Standard_E4s_v3&nbsp;<sup>2</sup> | 4      | 32          | 64             | 8              | 8,000 / 64 (100)                                                      | 6,400 / 96                                | 2 / 2,000                                   |
| Standard_E8s_v3&nbsp;<sup>2</sup> | 8      | 64          | 128            | 16             | 16,000 / 128 (200)                                                    | 12,800 / 192                              | 4 / 4,000                                       |
| Standard_E16s_v3&nbsp;<sup>2</sup> | 16     | 128         | 256            | 32             | 32,000 / 256 (400)                                                    | 25,600 / 384                              | 8 / 8,000                                       |
| Standard_E32s_v3&nbsp;<sup>2</sup> | 32     | 256         | 512            | 32             | 64,000 / 512 (800)                                                    | 51,200 / 768                              | 8 / 16,000                             |
| Standard_E64s_v3&nbsp;<sup>2</sup> | 64     | 432         | 8
64            | 32             | 128,000/1024 (1600)                                                   | 80,000 / 1200                             | 8 / 30,000                             |
| Standard_E64is_v3&nbsp;<sup>3</sup> | 64     | 432         | 8
64            | 32             | 128,000/1024 (1600)                                                   | 80,000 / 1200                             | 8 / 30,000                             |

<sup>1</sup> Esv3 系列 VM 的 Intel® 超线程技术功能。

<sup>2</sup> 受约束的可用核心大小。

<sup>3</sup> 实例与专用于单个客户的硬件隔离。
<!--PENDIND ON Esv3-series, Updte carefully -->

<!--PENDIND ON Ev3-series, Updte carefully -->
## <a name="ev3-series-sup1sup"></a>Ev3 系列 <sup>1</sup>

ACU：160 - 190 

Ev3 系列实例基于 2.3 GHz Intel XEON® E5-2673 v4 (Broadwell) 处理器，可通过 Intel Turbo Boost Technology 2.0 达到 3.5 GHz。 Ev3 系列实例适用于内存密集型企业应用程序。

数据磁盘存储与虚拟机分开计费。 若要使用高级存储磁盘，请使用 ESv3 大小。 ESv3 系列大小的定价和计费标准与 Ev3 系列相同。 

| 大小            | vCPU | 内存：GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 临时存储的最大吞吐量：IOPS/读取 MBps/写入 MBps | 最大网卡数/网络带宽等级 |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1,000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2,000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4,000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8,000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16,000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30,000           |
| Standard_E64i_v3&nbsp;<sup>2</sup> | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30,000           |

<sup>1</sup> Ev3 系列 VM 的 Intel® 超线程技术功能。

<sup>2</sup> 受约束的可用核心大小。 
<!--PENDIND ON Ev3-series, Updte carefully -->
<!-- Not Available on ## M-series <sup>1</sup> -->
<!-- Not Available on ## GS-series <sup>1</sup> -->
<!-- Not Available ## G-series-->
## <a name="dsv2-series-sup1sup"></a>DSv2 系列 <sup>1</sup>
<!-- NOTICE: 最大 NIC 数/预期网络带宽 (Mbps) SHOULD BE (Mbps) -->
ACU：210 - 250

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 缓存和临时存储的最大吞吐量：IOPS/MBps（以 GiB 为单位的缓存大小） | 非缓存磁盘最大吞吐量：IOPS / MBps | 最大 NIC 数/预期网络带宽 (MBps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2&nbsp;<sup>3</sup> |2 |14 |28 |8 |8,000 / 64 (72) |6,400 / 96 |2 / 1500 |
| Standard_DS12_v2&nbsp;<sup>3</sup> |4 |28 |56 |16 |16,000 / 128 (144) |12,800 / 192 |4 / 3000 |
| Standard_DS13_v2&nbsp;<sup>3</sup> |8 |56 |112 |32 |32,000 / 256 (288) |25,600 / 384 |8 / 6000 |
| Standard_DS14_v2&nbsp;<sup>3</sup>|16 |112 |224 |64 |64,000 / 512 (576) |51,200 / 768 |8 / 12000 |
| Standard_DS15_v2&nbsp;<sup>2</sup> |20 个 |140 |280 |64 |80,000 / 640 (720) |64,000 / 960 |8 / 25000&nbsp;<sup>4</sup>
<!-- Please acknowledge that DSv2 Max Disk Count are 8,16,32,64,64 -->

<sup>1</sup> DSv2 系列 VM 可能的最大磁盘吞吐量（IOPS 或 MBps）可能受限于附加磁盘的数量、大小和条带化。  有关详细信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../articles/virtual-machines/windows/premium-storage.md)。

<sup>2</sup> 实例对于专用于单个客户的硬件独立。

<sup>3</sup> 受约束的可用核心大小。

<sup>4</sup> 25000 Mbps，具有加速网络。

<br>

## <a name="dv2-series"></a>Dv2 系列

ACU：210 - 250

| 大小              | vCPU | 内存：GiB | 临时存储 (SSD) GiB | 临时存储的最大吞吐量：IOPS/读取 MBps/写入 MBps | 最大的数据磁盘/吞吐量：IOPS | 最大 NIC 数/预期网络带宽 (MBps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 8/8x500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64/64x500                       | 8 / 12000          |
| Standard_D15_v2&nbsp;<sup>1</sup> | 20 个        | 140         | 1,000          | 60000/937/468                                        | 64/64x500                       | 8 / 25000&nbsp;<sup>2</sup> |
<!-- Please acknowledge that Dv2 Max Disk Count are 8,16,32,64,64 -->

<sup>1</sup> 实例对于专用于单个客户的硬件独立。 

<sup>2</sup> 25000 Mbps，具有加速网络。

<br>

## <a name="ds-series-sup1sup"></a>DS 系列 <sup>1</sup>

ACU：160

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 缓存和临时存储的最大吞吐量：IOPS/MBps（以 GiB 为单位的缓存大小） | 非缓存磁盘最大吞吐量：IOPS / MBps | 最大 NIC 数/预期网络带宽 (MBps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |8 |8,000 / 64 (72) |6,400 / 64 |2 / 1000 |
| Standard_DS12 |4 |28 |56 |16 |16,000 / 128 (144) |12,800 / 128 |4 / 2000 |
| Standard_DS13 |8 |56 |112 |32 |32,000 / 256 (288) |25,600 / 256 |8 / 4000 |
| Standard_DS14 |16 |112 |224 |64 |64,000 / 512 (576) |51,200 / 512 |8 / 8000 |
<!-- Please acknowledge that DS Max Disk Count are 8,16,32,64 -->

<sup>1</sup> DS 系列 VM 可能的最大磁盘吞吐量（IOPS 或 MBps）可能受限于附加磁盘的数量、大小和条带化。  有关详细信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../articles/virtual-machines/windows/premium-storage.md)。

## <a name="d-series"></a>D 系列

ACU：160

| 大小         | vCPU | 内存：GiB | 临时存储 (SSD) GiB | 临时存储的最大吞吐量：IOPS/读取 MBps/写入 MBps | 最大的数据磁盘/吞吐量：IOPS | 最大 NIC 数/预期网络带宽 (MBps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000/93/46                                           | 8/8x500                         | 2 / 1000                     |
| Standard_D12 | 4         | 28          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 2000                     |
| Standard_D13 | 8         | 56          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 4000                     |
| Standard_D14 | 16        | 112         | 800            | 48000/750/375                                        | 64/64x500                       | 8 / 8000                |
<!-- Please acknowledge that D-series Max Disk Count are 8,16,32,64-->
<!-- NOTICE: 最大 NIC 数/预期网络带宽 (Mbps) SHOULD BE (Mbps) -->

<br>
<!--Update_Description: wording update, update link -->
<!--PENDING to EV3 configuration list-->