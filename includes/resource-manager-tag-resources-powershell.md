AzureRm.Resources 模块版本 3.0 包括在使用标记方式方面的重大更改。 在继续之前，请查看版本：

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

如果结果显示版本 3.0 或更高版本，则本主题中的示例可用于环境。 如果没有版本 3.0 或更高版本，请使用 PowerShell 库或 Web 平台安装程序 [更新版本](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) ，再继续完成本主题。

```powershell
Version
-------
3.5.0
```

若要查看**资源组**的现有标记，请使用：

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

会返回以下格式：

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

若要查看**具有指定资源 ID 的资源**的现有标记，请使用：

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

或者，若要查看**具有指定名称的资源以及资源组**的现有标记，请使用：

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

若要获取**具有特定标记的资源组**，请使用：

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

若要获取**具有特定标记的资源**，请使用：

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

每次将标记应用到资源或资源组时，都会覆盖该资源或资源组上的现有标记。 因此，必须根据该资源或资源组是否包含现有标记来使用不同的方法。 

若要将标记添加到**不包含现有标记的资源组**，请使用：

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

若要将标记添加到**包含现有标记的资源组**，请检索现有标记，添加新标记，并重新应用标记：

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

若要将标记添加到**不包含现有标记的资源**，请使用：

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName exampleroup
```

将标记添加到**包含现有标记的资源**。

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
```

要将资源组中的所有标记应用于其资源，并且 **不保留资源上的现有标记**，请使用以下脚本：

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

要将资源组中的所有标记应用于其资源，并且 **保留资源上不重复的现有标记**，请使用以下脚本：

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    if ($g.Tags -ne $null) {
        $resources = Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName 
        foreach ($r in $resources)
        {
            $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
            foreach ($key in $g.Tags.Keys)
            {
                if ($resourcetags.ContainsKey($key)) { $resourcetags.Remove($key) }
            }
            $resourcetags += $g.Tags
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
    }
}
```

若要删除所有标记，请传递一个空哈希表。

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```
<!--Update_Description: wording update, update the source code-->