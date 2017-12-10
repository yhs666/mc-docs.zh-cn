<!-- Not Available M-Series -->
* Dv2 系列和 D 系列最适用于要求有更快速的 vCPU、更好的临时存储性能或更高内存的应用程序。  它们为许多企业级应用程序提供强大的组合。

* D 系列的 VM 旨在运行需要更高计算能力和临时磁盘性能的应用程序。 D 系列 VM 为临时存储提供更快的处理器、更高的内存 vCPU 比和固态硬盘 (SSD)。 有关详细信息，请参阅 Azure 博客[新的 D 系列虚拟机大小](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)上的公告。

* Dv2 系列是原 D 系列的后续系列，其特点是 CPU 功能更强大。 Dv2 系列 CPU 比 D 系列 CPU 快大约 35%。 该系列基于最新一代的 2.4 GHz Intel Xeon® E5-2673 v3 (Haswell) 处理器，通过 Intel Turbo Boost Technology 2.0 可以达到 3.1 GHz。 Dv2 系列的内存和磁盘配置与 D 系列相同。

<!-- Not Available ## ESv3-series -->
<!-- Not Available ## Ev3-series-->
<!-- Not Available ## M-series* -->
<!-- Not Available ## GS-series*-->
<!-- Not Available ## G-series-->
## <a name="dsv2-series"></a>DSv2 系列*

ACU：210 - 250

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 缓存和临时存储的最大吞吐量：IOPS/MBps（以 GiB 为单位的缓存大小） | 非缓存磁盘最大吞吐量：IOPS / MBps | 最大 NIC 数/预期网络性能 (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2 |2 |14 |28 |8 |8,000 / 64 (72) |6,400 / 96 |2 / 1500 |
| Standard_DS12_v2 |4 |28 |56 |16 |16,000 / 128 (144) |12,800 / 192 |4 / 3000 |
| Standard_DS13_v2 |8 |56 |112 |32 |32,000 / 256 (288) |25,600 / 384 |8 / 6000 |
| Standard_DS14_v2 |16 |112 |224 |64 |64,000 / 512 (576) |51,200 / 768 |8 / 6000 - 12000 &#8224; |
| Standard_DS15_v2** |20 |140 |280 |64 |80,000 / 640 (720) |64,000 / 960 |8 / 20000***|
<!-- Please acknowledge that DSv2 Max Disk Count are 8,16,32,64,64 -->

*DSv2 系列 VM 可能的最大磁盘吞吐量（IOPS 或 MBps）可能受限于附加磁盘的数量、大小和条带化。  有关详细信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../articles/storage/common/storage-premium-storage.md)。

**实例是一个独立的节点，可保证你的 VM 是 Intel Haswell 节点上的唯一 VM。

***25000 Mbps，具有加速网络。

<br>

## <a name="dv2-series"></a>Dv2 系列

ACU：210 - 250

| 大小              | vCPU | 内存：GiB | 临时存储 (SSD) GiB | 临时存储的最大吞吐量：IOPS/读取 MBps/写入 MBps | 最大的数据磁盘/吞吐量：IOPS | 最大 NIC 数/预期网络性能 (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 8/4x500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/8x500                        | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/16x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64/32x500                       | 8 / 6000 - 12000 &#8224;          |
| Standard_D15_v2*  | 20        | 140         | 1,000          | 60000/937/468                                        | 64/40x500                       | 8 / 20000** |
<!-- Please acknowledge that Dv2 Max Disk Count are 8,16,32,64,64 -->

*实例是一个独立的节点，可保证 VM 是 Intel Haswell 节点上的唯一 VM。

**25000 Mbps，具有加速网络。

<br>

## <a name="ds-series"></a>DS 系列*

ACU：160

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 缓存和临时存储的最大吞吐量：IOPS/MBps（以 GiB 为单位的缓存大小） | 非缓存磁盘最大吞吐量：IOPS / MBps | 最大 NIC 数/预期网络性能 (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |8 |8,000 / 64 (72) |6,400 / 64 |2 / 1000 |
| Standard_DS12 |4 |28 |56 |16 |16,000 / 128 (144) |12,800 / 128 |4 / 2000 |
| Standard_DS13 |8 |56 |112 |32 |32,000 / 256 (288) |25,600 / 256 |8 / 4000 |
| Standard_DS14 |16 |112 |224 |64 |64,000 / 512 (576) |51,200 / 512 |8 / 6000 - 8000 &#8224; |
<!-- Please acknowledge that DS Max Disk Count are 8,16,32,64 -->

*DS 系列 VM 可能的最大磁盘吞吐量（IOPS 或 MBps）可能受限于附加磁盘的数量、大小和条带化。  有关详细信息，请参阅[高级存储：适用于 Azure 虚拟机工作负荷的高性能存储](../articles/storage/common/storage-premium-storage.md)。

## <a name="d-series"></a>D 系列

ACU：160

| 大小         | vCPU | 内存：GiB | 临时存储 (SSD) GiB | 临时存储的最大吞吐量：IOPS/读取 MBps/写入 MBps | 最大的数据磁盘/吞吐量：IOPS | 最大 NIC 数/预期网络性能 (Mbps) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000/93/46                                           | 8/4x500                         | 2 / 1000                     |
| Standard_D12 | 4         | 28          | 200            | 12000/187/93                                         | 16/8x500                         | 4 / 2000                     |
| Standard_D13 | 8         | 56          | 400            | 24000/375/187                                        | 32/16x500                       | 8 / 4000                     |
| Standard_D14 | 16        | 112         | 800            | 48000/750/375                                        | 64/32x500                       | 8 / 6000 - 8000 &#8224;                |
<!-- Please acknowledge that D-series Max Disk Count are 8,16,32,64-->

<br>
<!--Update_Description: wording update-->
<!--ms.date: 12/11/2017-->