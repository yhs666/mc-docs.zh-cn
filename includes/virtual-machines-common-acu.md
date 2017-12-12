我们已创建 Azure 计算单元 (ACU)，提供一种比较 Azure SKU 的计算 (CPU) 性能的方法。 这有助于轻松确定最有可能满足性能需求的 SKU。  ACU 目前在小型 (Standard_A1) VM 上标准为 100，而所有其他 SKU 表示 SKU 在运行标准基准测试时大约可以有多快。 

> [!IMPORTANT]
> ACU 只是一种规则。  工作负荷的结果可能会有所不同。 
> 
> 

<br>

| SKU 系列 | ACU \ vCPU |
| --- | --- |
| [A0](../articles/virtual-machines/windows/sizes-general.md) |50 |
| [A1-A4](../articles/virtual-machines/windows/sizes-general.md) |100 个 |
| [A5-A7](../articles/virtual-machines/windows/sizes-general.md) |100 |
| [A1_v2-A8_v2](../articles/virtual-machines/windows/sizes-general.md) |100 |
| [A2m_v2-A8m_v2](../articles/virtual-machines/windows/sizes-general.md) |100 |
<!-- Not Available  [A8-A11]  -->
| [D1-D14](../articles/virtual-machines/windows/sizes-general.md) |160 | | [D1_v2-D15_v2](../articles/virtual-machines/windows/sizes-general.md) |210 - 250* | | [DS1-DS14](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 | | [DS1_v2-DS15_v2](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |210-250* | | [F1-F16](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | | [F1s-F16s](../articles/virtual-machines/windows/sizes-compute.md) |210-250* |
<!-- Not Available  [G1-G5]  -->
<!-- Not Available  [GS1-GS5]  -->
<!-- Not Available  [H] -->
<!-- Not Available  [L4s-L32s]  -->
<!-- Not Available  [M] -->

ACU 标有 *使用 Intel® Turbo 技术来增加 CPU 频率，并提升性能。  提升量可能因 VM 大小、工作负荷和同一主机上运行的其他工作负荷而有所不同。

**超线程。

<!-- ms.date: 12/11/2017 -->
<!--Update_Description: wording update-->