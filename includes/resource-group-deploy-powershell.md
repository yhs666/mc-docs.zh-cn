## 如何使用 PowerShell 进行部署
<a id="how-to-deploy-with-powershell" class="xliff"></a>

1. 登录到 Azure 帐户。

    ```
      Add-AzureAccount
    ```

   提供凭据后，该命令将返回有关你的帐户的信息。

   ```
      Id                             Type       ...
      --                             ----    
      example@contoso.com            User       ...   
   ```

2. 如果你有多个订阅，请提供要用于部署的订阅 ID。 

    ```
      Select-AzureSubscription -SubscriptionID <YourSubscriptionId>
    ```

3. 切换到“Azure Resource Manager”模块。

    ```
      Switch-AzureMode AzureResourceManager
    ```

4. 如果目前没有资源组，请创建新的资源组。 提供资源组的名称，以及解决方案所需的位置。

    ```
    New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"
    ```

   将返回新资源组的摘要。

   ```
    ResourceGroupName : ExampleResourceGroup
    Location          : China North
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                Actions  NotActions
                =======  ==========
                *
    ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup
   ```

5. 若要为资源组创建新部署，请运行 **New-AzureResourceGroupDeployment** 命令并提供所需的参数。 参数包括部署的名称、资源组的名称、所创建模板的路径或 URL，以及方案所需的任何其他参数。 

   可以使用以下选项提供参数值： 

   - 使用内联参数。

       ```
        New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"
       ```

   - 使用参数对象。

       ```
        $parameters = @{"<ParameterName>"="<Parameter Value>"}
        New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters
       ```

   - 使用参数文件。

       ```
        New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>
       ```

  部署资源组后，将看到部署摘要。

  ```
         DeploymentName    : ExampleDeployment
         ResourceGroupName : ExampleResourceGroup
         ProvisioningState : Succeeded
         Timestamp         : 4/14/2015 7:00:27 PM
         Mode              : Incremental
         ...
  ```

6. 获取有关部署错误的信息。

    ```
    Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed
    ```

7. 获取有关部署错误的详细信息。

    ```
    Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
    ```