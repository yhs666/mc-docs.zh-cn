> [!NOTE]
> .publishsettings 文件中包含用于管理 Azure 订阅和服务的凭据（未编码）。 保护此文件的最佳做法是，将其暂时存储在源目录的外部（例如存储在 Libraries\Documents 文件夹中），在完成导入后将其删除。 获得了 .publishsettings 文件访问权的恶意用户可以编辑、创建和删除您的 Azure 服务。