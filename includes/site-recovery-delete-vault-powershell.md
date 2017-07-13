## 删除恢复服务保管库 (PowerShell)
<a id="delete-a-recovery-services-vault-powershell" class="xliff"></a>

1. 获取恢复服务保管库

    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name "ContosoVault"
    ```

2. 删除保管库

    ```
    Remove-AzureRmRecoveryServicesVault -Vault $vault
    ```

>[!WARNING]
>
> 请务必谨慎使用上述命令，因为如果误删任何保管库，将丢失所有数据。 这是一种永久性操作且不可逆转。