## <a name="overview-of-azure-resource-manager-templates"></a>Azure Resource Manager 模板概述

>[!NOTE]
> 必须修改从 GitHub 存储库“azure-quickstart-templates”下载的模板，以适应 Azure 中国云环境。 例如，替换某些终结点（将“blob.core.windows.net”替换为“blob.core.chinacloudapi.cn”，将“cloudapp.azure.com”替换为“chinacloudapp.cn”）；更改某些不受支持的 VM 映像；更改某些不受支持的 VM 大小。

Azure Resource Manager 模板允许用户通过定义资源之间的依赖关系，使用 JSON 语言以声明方式指定 Azure IaaS 基础结构。 有关 Azure Resource Manager 模板的详细概述，请参阅以下文章：

[资源组概述](../articles/azure-resource-manager/resource-group-overview.md)

## <a name="sample-template-snippet-for-vm-extensions"></a>VM 扩展的示例模板片段
将 VM 扩展部署为 Azure Resource Manager 模板的一部分要求用户在模板中以声明方式指定扩展配置。
以下是用于指定扩展配置的格式。

      {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "MyExtension",
      "apiVersion": "2015-05-01-preview",
      "location": "[parameters('location')]",
      "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
      "properties":
      {
      "publisher": "Publisher Namespace",
      "type": "extension Name",
      "typeHandlerVersion": "extension version",
      "settings": {
      // Extension specific configuration goes in here.
      }
      }
      }

如上所示，扩展模板包含两个主要部分：

1. 扩展名称、发布者和版本
2. 扩展配置。

## <a name="identifying-the-publisher-type-and-typehandlerversion-for-any-extension"></a>确定任何扩展的发布者、类型和 typeHandlerVersion
Azure VM 扩展由 Microsoft 和受信任的第三方发布者发布，每个扩展都通过其发布者、类型和 typeHandlerVersion 进行唯一标识。 可通过以下方式确定这些内容：