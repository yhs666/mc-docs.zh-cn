# <a name="using-managed-disks-in-azure-resource-manager-templates"></a>在 Azure Resource Manager 模板中使用托管磁盘

本文介绍使用 Azure Resource Manager 模板预配虚拟机时托管与非托管磁盘之间的差异。 这有助于将现有模板从使用非托管磁盘更新为使用托管磁盘。 我们将使用 [101-vm-simple-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) 模板作为参考指南。 如果想要直接对它们进行比较，可以同时看到使用[托管磁盘](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json)的模板和以前使用[非托管磁盘](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json)的版本。

## <a name="unmanaged-disks-template-formatting"></a>非托管磁盘模板的格式设置

首先，了解如何部署非托管磁盘。 创建非托管磁盘时，需要一个存储帐户用来保留 VHD 文件。 可新建一个存储帐户或使用已存在的帐户。 本文将说明如何新建存储帐户。 为实现此目的，资源块中需要如下所示的存储帐户资源。

```
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2016-01-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

在虚拟机对象内，存储帐户上需要具有一个依赖项，确保它创建在虚拟机之前。 随后在 `storageProfile` 部分中，指定 VHD 位置的完整 URI，用于引用存储帐户并且 OS 磁盘和任何数据磁盘都需要它。 

```
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "name": "osdisk",
                "vhd": {
                    "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "name": "datadisk1",
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "vhd": {
                        "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/datadisk1.vhd')]"
                    },
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

## <a name="managed-disks-template-formatting"></a>托管磁盘模板的格式设置

若使用 Azure 托管磁盘，磁盘将成为顶级资源，不再需要用户创建存储帐户。 托管磁盘在 `2016-04-30-preview` API 版本中首次公开，并在所有后续 API 版本中可用，现在是默认磁盘类型。 以下各部分将讲解默认设置，并详细介绍如何进一步自定义磁盘。

> [!NOTE]
> 建议使用 `2016-04-30-preview` 以后的 API 版本，因为在 `2016-04-30-preview` 和 `2017-03-30` 之间存在重大更改。
>
>

### <a name="default-managed-disk-settings"></a>默认托管磁盘设置

若要创建带托管磁盘的 VM，无需再创建存储帐户资源，可如下所示更新虚拟机资源。 特别要注意，`apiVersion` 反映 `2017-03-30`，并且 `osDisk` 和 `dataDisks` 不再为 VHD 引用特定 URI。 如果部署时未指定其他属性，磁盘将使用[标准 LRS 存储](../articles/storage/common/storage-redundancy.md)。 如果未指定任何名称，则 OS 磁盘采用格式 `<VMName>_OsDisk_1_<randomstring>`，每个数据磁盘采用格式 `<VMName>_disk<#>_<randomstring>`。 默认情况下，Azure 磁盘加密处于禁用状态；缓存对于 OS 磁盘为“读/写”，对于数据磁盘则为“无”。 你可能会注意到以下示例中仍然存在一个存储帐户依赖项，但这仅用于诊断的存储，磁盘存储并不需要。

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="using-a-top-level-managed-disk-resource"></a>使用顶级托管磁盘资源

在虚拟机对象中指定磁盘配置的另一种选择是，创建顶级磁盘资源，并在虚拟机创建过程中进行附加。 例如，可如下所示创建磁盘资源作为数据磁盘使用。

```
{
    "type": "Microsoft.Compute/disks",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "apiVersion": "2017-03-30",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "properties": {
        "creationData": {
            "createOption": "Empty"
        },
        "diskSizeGB": 1023
    }
}
```

随后可在该 VM 对象内引用此要附加的磁盘对象。 创建 VM 时，通过指定在 `managedDisk` 属性中创建的托管磁盘的资源 ID，允许附加磁盘。 请注意，VM 资源的 `apiVersion` 设置为 `2017-03-30`。 另请注意，我们已在磁盘资源上创建了一个依赖项，确保它在创建 VM 之前已成功创建。 

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]",
        "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "lun": 0,
                    "name": "[concat(variables('vmName'),'-datadisk1')]",
                    "createOption": "attach",
                    "managedDisk": {
                        "id": "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
                    }
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a>通过使用托管磁盘的 VM 创建托管可用性集

若要通过使用托管磁盘的 VM 创建托管可用性集，请将 `sku` 对象添加到可用性集资源中，并将 `name` 属性设置为 `Aligned`。 这可确保每个 VM 的磁盘彼此充分隔离，避免出现单点故障。 另请注意，可用性集资源的 `apiVersion` 设置为 `2017-03-30`。

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/availabilitySets",
    "location": "[resourceGroup().location]",
    "name": "[variables('avSetName')]",
    "properties": {
        "PlatformUpdateDomainCount": 3,
        "PlatformFaultDomainCount": 2
    },
    "sku": {
        "name": "Aligned"
    }
}
```

### <a name="additional-scenarios-and-customizations"></a>其他方案和自定义项

若要查找有关 REST API 规范的完整信息，请查看[创建托管磁盘 REST API 文档](https://docs.microsoft.com/rest/api/manageddisks/disks/disks-create-or-update)。 该文档介绍了其他方案以及可通过模板部署提交到 API 的默认值和可接受的值。 

## <a name="next-steps"></a>后续步骤

* 有关使用托管磁盘的完整模板，请访问以下 Azure 快速入门存储库链接。
    * [带托管磁盘的 Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [带托管磁盘的 Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [托管磁盘模板的完整列表](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* 请访问文章 [Azure 托管磁盘概述](../articles/virtual-machines/windows/managed-disks-overview.md)，详细了解托管磁盘。
* 访问文档 [Microsoft.Compute/virtualMachines template reference](https://docs.microsoft.com/templates/microsoft.compute/virtualmachines)（Microsoft.Compute/virtualMachines 模板参考），查看虚拟机资源的模板参考文档。
* 访问文档 [ template reference](https://docs.microsoft.com/templates/microsoft.compute/disks)（Microsoft.Compute/disks 模板参考），查看虚拟机资源的模板参考文档。
 
