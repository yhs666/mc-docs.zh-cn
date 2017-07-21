你必须为每个托管 Azure 副本的 VM 创建一个负载均衡的终结点。 如果你在多个区域中拥有副本，该区域的每个副本必须位于同一个 VNet 的同一个云服务中。 跨越多个 Azure 区域创建可用性组副本需要配置多个 Vnet。 有关配置跨 VNet 连接的详细信息，请参阅[配置 VNet 到 VNet 连接](../articles/vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)。

1. 在 Azure 经典管理门户中，导航到托管副本的每个 VM 并查看详细信息。
2. 单击每个 VM 的“终结点”选项卡。
3. 确保想要使用的侦听器终结点“名称”和“公用端口”未被使用。 在下面的示例中，名称为“MyEndpoint”，端口为“1433”。
4. 在你本地的客户端上，下载并安装 [最新的 PowerShell 模块](/downloads/)。
5. 启动 **Azure PowerShell**。 随即打开新的 PowerShell 会话，其中加载了 Azure 管理模块。
6. 运行 **Get-AzurePublishSettingsFile**。 此 cmdlet 会定向到浏览器，以将发布设置文件下载到本地目录。 系统可能会提示输入 Azure 订阅的登录凭据。
7. 运行 Import-azurepublishsettingsfile 命令以及你下载发布设置文件的路径： 

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    导入发布设置文件后，便可以在 PowerShell 会话中管理 Azure 订阅。