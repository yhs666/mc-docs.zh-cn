## <a name="sign-in-to-azure"></a>登录 Azure

运行 `Connect-AzureRmAccount -EnvironmentName AzureChinaCloud` 命令以登录 Azure 订阅，并按照屏幕上的说明操作。

```powershell
Connect-AzureRmAccount -EnvironmentName AzureChinaCloud
```

如果你不知道要使用哪个位置，可以列出可用的位置。 显示列表后，找到要使用的位置。 此示例使用“中国东部”。 将其存储在变量中，并使用该变量，这样就可以在某个位置更改它。

```powershell
Get-AzureRmLocation | select Location 
$location = "China East"
```

## <a name="create-a-resource-group"></a>创建资源组

使用 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) 创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-storage-account"></a>创建存储帐户

使用 [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount) 创建具有 LRS 复制的标准常规用途存储帐户，然后检索定义要使用的存储帐户的存储帐户上下文。 对存储帐户执行操作时，引用上下文而不是重复提供凭据。 本示例创建一个名为 mystorageaccount 的存储帐户，默认启用了本地冗余存储 (LRS) 和 Blob 加密。

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```
