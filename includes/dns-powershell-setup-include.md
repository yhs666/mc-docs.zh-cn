## <a name="set-up-azure-powershell-for-azure-dns"></a>设置适用于 Azure DNS 的 Azure PowerShell

### <a name="before-you-begin"></a>准备阶段

在开始配置之前，请确保具备以下各项。

* Azure 订阅。 如果还没有 Azure 订阅，可在开始前创建一个 [1 元人民币的试用](https://www.azure.cn/pricing/1rmb-trial/)帐户。
* 需安装最新版本的 Azure 资源管理器 PowerShell cmdlet。 有关详细信息，请参阅[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs)。

### <a name="sign-in-to-your-azure-account"></a>登录到 Azure 帐户

打开 PowerShell 控制台并连接到帐户。 有关详细信息，请参阅[将 PowerShell 与 Resource Manager 配合使用](../articles/azure-resource-manager/powershell-azure-resource-manager.md)。

```powershell
Connect-AzureRmAccount -EnvironmentName AzureChinaCloud
```

### <a name="select-the-subscription"></a>选择订阅
 
检查该帐户的订阅。

```powershell
Get-AzureRmSubscription
```

选择要使用的 Azure 订阅。

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>创建资源组

Azure Resource Manager 要求所有资源组指定一个位置。 此位置用作该资源组中的资源的默认位置。 但是，由于所有 DNS 资源都是全局性而非区域性的，因此资源组位置的选择不会影响 Azure DNS。

如果使用现有资源组，可跳过此步骤。

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "China East"
```

### <a name="register-resource-provider"></a>注册资源提供程序

Azure DNS 服务由 Microsoft.Network 资源提供程序管理。 使用 Azure DNS 前，必须将 Azure 订阅注册为使用此资源提供程序。 对每个订阅而言，这都是一次性操作。

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```