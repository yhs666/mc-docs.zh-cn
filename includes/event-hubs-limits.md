下表列出了特定于 [Azure 事件中心](https://www.azure.cn/home/features/event-hubs/)的配额和限制。 有关事件中心定价的信息，请参阅[事件中心定价](https://www.azure.cn/pricing/details/event-hubs/)。

| 限制 | 作用域 | 注释 | 值 |
| ----- | ----- | ----- | ----- |
| 每个命名空间的事件中心数 |命名空间 |创建新事件中心的后续请求会被拒绝。 |10 个 |
| 每个事件中心的分区数 |实体 |- |32 |
| 每个事件中心的使用者组数 |实体 |- |20 个 |
| 每个命名空间的 AMQP 连接数 |命名空间 |系统将拒绝后续的附加连接请求，且调用代码会收到异常。 |5,000 |
| 事件中心事件的最大大小|实体 |- |256 KB |
| 事件中心名称的最大大小 |实体 |- |50 个字符 |
| 每个使用者组的非 epoch 接收者数 |实体 |- |5 |
| 事件数据的最长保留期限 |实体 |- |1-7 天 |
| 最大吞吐量单位 |命名空间 |超出吞吐量单位限制会导致数据受到限制，并生成 [ServerBusyException](https://docs.azure.cn/zh-cn/dotnet/api/microsoft.servicebus.messaging.serverbusyexception?view=azure-dotnet)。 [额外的吞吐量单位](../articles/event-hubs/event-hubs-auto-inflate.md)将基于承诺的购买以大小为 20 个单位的块的形式提供。 |20 个 |
<!-- Not Available on [support request](/azure-supportability/how-to-create-azure-support-request) -->
| 每个命名空间的授权规则数量 |命名空间|将拒绝后续的授权规则创建请求。|12 |
<!-- ms.date: 04/02/2017 -->
<!-- Update_Description: update meta properties -->