| 资源 | 默认限制 |
| --- | --- |
| 每个区域的存储帐户数 | 200<sup>1</sup> |
| 最大存储帐户容量 | 500 TiB<sup>2</sup> |
| 每个存储帐户的 Blob 容器、Blob、文件共享、表、队列、实体或消息数上限 | 无限制 |
| 每个存储帐户的最大请求速率 | 每秒 20,000 个请求<sup>2</sup> |
| 每个存储帐户的最大入口<sup>3</sup>（美国区域） | 如果已启用 RA-GRS/GRS，则为 10 Gbps；对于 LRS<sup>4</sup>，为 20 Gbps |
| 每个存储帐户的最大出口<sup>3</sup>（美国区域） | 如果已启用 RA-GRS/GRS，则为 20 Gbps；对于 LRS<sup>4</sup>，为 30 Gbps |
| 每个存储帐户的最大入口<sup>3</sup>（非美国区域） | 如果已启用 RA-GRS/GRS，则为 5 Gbps；对于 LRS<sup>4</sup>，为 10 Gbps |
| 每个存储帐户的最大出口<sup>3</sup>（非美国区域） | 如果已启用 RA-GRS/GRS，则为 10 Gbps；对于 LRS<sup>4</sup>，为 15 Gbps |

<sup>1</sup>包括标准存储帐户和高级存储帐户。 如果需要的存储帐户超过 200 个，请通过 [Azure 支持](https://www.azure.cn/support/contact/)提出请求。 Azure 存储团队将评审你的业务案例，最多可以批准 250 个存储帐户。 

<sup>2</sup> 如果需要扩大存储帐户的限制，请联系 [Azure 支持](https://www.azure.cn/support/contact/)。 Azure 存储团队会对请求进行审核，根据具体情况批准提高限制的请求。 通用和 Blob 存储帐户请求支持都可按请求提高容量、流入量/流出量和请求速率。 有关 Blob 存储帐户的新最大值，请参阅[宣布推出更大、更高规模的存储帐户](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/)。

<sup>3</sup>仅受帐户入口/出口限制的约束。 *入口*是指发送到存储帐户的所有数据（请求）。 “传出”是指从存储帐户接收的所有数据（响应）。  

<sup>4</sup>Azure 存储冗余选项包括：
* **RA-GRS**：读取访问异地冗余存储。 如果已启用 RA-GRS，辅助位置的出口目标与主要位置的出口目标相同。
* **GRS**：异地冗余存储空间。 
* **LRS**：本地冗余存储空间。 


