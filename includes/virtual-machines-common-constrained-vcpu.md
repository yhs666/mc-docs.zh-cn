某些数据库工作负荷（例如 SQL Server 或 Oracle）要求高内存、高存储和高 I/O 带宽，但不要求高核心计数。 许多数据库工作负荷不是 CPU 密集型的。 Azure 提供特定的 VM 大小，允许你对 VM vCPU 计数进行限制，在保留相同的内存、存储和 I/O 带宽的同时，降低软件许可成本。

可以将 vCPU 计数限制为原始 VM 大小的一半或四分之一。 这些新的 VM 大小有一个用于指定活动 vCPU 数的后缀，方便你对其进行标识。
<!-- Not Available on GS series -->

对 SQL Server 或 Oracle 收取的许可费用受限于新 vCPU 计数，其他产品的费用应以新 vCPU 计数为基础。 这会导致 VM 规格相对于活动（可计费）vCPU 的比率增加 50% 到 75%。 这些新的 VM 大小仅在 Azure 中提供，可以让工作负荷提高 CPU 利用率，所需费用只是许可费用（按核心计）的一部分。 目前的计算费用（包括 OS 许可）仍与原始大小一样。 有关详细信息，请参阅 [Azure VM sizes for more cost-effective database workloads](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/)（数据库工作负荷性价比更高的 Azure VM 大小）。

| Name                | vCPU | 规格           |
|---------------------|------|-----------------|
| Standard_DS13-4_v2  | 4    | 同 DS13_v2 |
| Standard_DS13-2_v2  | 2    | 同 DS13_v2 |
| Standard_DS14-8_v2  | 8    | 同 DS14_v2 |
| Standard_DS14-4_v2  | 4    | 同 DS14_v2 |
<!-- Not Available on Standard M, E, GS seriese -->
<!-- Update_Description: new articles on VM common constrained vCPU -->
<!-- ms.date: 01/08/2018 -->