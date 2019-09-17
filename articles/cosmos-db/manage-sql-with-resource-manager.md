---
title: 使用 Azure 资源管理器模板创建和管理 Azure Cosmos DB
description: 使用 Azure 资源管理器模板创建和配置 Azure Cosmos DB for SQL (Core) API
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 08/05/2019
ms.date: 09/09/2019
ms.author: v-yeche
ms.openlocfilehash: a6510a874d555ff44d97eade4382b6ee6c76eab0
ms.sourcegitcommit: 66192c23d7e5bf83d32311ae8fbb83e876e73534
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70254641"
---
<!--Verify successfully-->
# <a name="manage-azure-cosmos-db-sql-core-api-resources-using-azure-resource-manager-templates"></a>使用 Azure 资源管理器模板管理 Azure Cosmos DB SQL (Core) API 资源

<a name="create-resource"></a>
## <a name="create-an-azure-cosmos-account-database-and-container"></a>创建 Azure Cosmos 帐户、数据库和容器 

使用 Azure 资源管理器模板创建 Azure Cosmos DB 资源。 此模板将创建 Azure Cosmos 帐户，所使用的两个容器在数据库级别共享 400 RU/秒的吞吐量。 复制模板并按如下所示进行部署，或者访问 [Azure 快速入门库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-sql/)，然后从 Azure 门户进行部署。 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

> [!NOTE]
>
> - 当前，无法使用资源管理器模板部署用户定义函数 (UDF)、存储过程和触发器。
> - 不能同时在 Azure Cosmos 帐户中添加或删除位置，也不能修改其他属性。 这些操作必须作为单独的操作完成。
> - 帐户名称必须为小写且 < 31 个字符。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "defaultValue": "[concat('sql-', uniqueString(resourceGroup().id))]",
            "metadata": {
                "description": "Cosmos DB account name"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for the Cosmos DB account."
            }
        },
        "primaryRegion":{
            "type":"string",
            "metadata": {
                "description": "The primary replica region for the Cosmos DB account."
            }
        },
        "secondaryRegion":{
            "type":"string",
            "metadata": {
              "description": "The secondary replica region for the Cosmos DB account."
          }
        },
        "defaultConsistencyLevel": {
            "type": "string",
            "defaultValue": "Session",
            "allowedValues": [ "Eventual", "ConsistentPrefix", "Session", "BoundedStaleness", "Strong" ],
            "metadata": {
                "description": "The default consistency level of the Cosmos DB account."
            }
        },
        "maxStalenessPrefix": {
            "type": "int",
            "minValue": 10,
            "defaultValue": 100000,
            "maxValue": 2147483647,
            "metadata": {
                "description": "Max stale requests. Required for BoundedStaleness. Valid ranges, Single Region: 10 to 1000000. Multi Region: 100000 to 1000000."
            }
        },
        "maxIntervalInSeconds": {
            "type": "int",
            "minValue": 5,
            "defaultValue": 300,
            "maxValue": 86400,
            "metadata": {
                "description": "Max lag time (seconds). Required for BoundedStaleness. Valid ranges, Single Region: 5 to 84600. Multi Region: 300 to 86400."
            }
        },  
        "multipleWriteLocations": {
            "type": "bool",
            "defaultValue": true,
            "allowedValues": [ true, false ],
            "metadata": {
                "description": "Enable multi-master to make all regions writable."
            }
        },
        "automaticFailover": {
            "type": "bool",
            "defaultValue": false,
            "allowedValues": [ true, false ],
            "metadata": {
                "description": "Enable automatic failover for regions. Ignored when Multi-Master is enabled"
            }
        },
        "databaseName": {
            "type": "string",
            "metadata": {
                "description": "The name for the SQL database"
            }
        },
        "throughput": {
            "type": "int",
            "defaultValue": 400,
            "minValue": 400,
            "maxValue": 1000000,
            "metadata": {
                "description": "The throughput for the database"
            }           
        },
        "container1Name": {
            "type": "string",
            "defaultValue": "container1",
            "metadata": {
                "description": "The name for the first SQL container"
            }
        },
        "container2Name": {
            "type": "string",
            "defaultValue": "container2",
            "metadata": {
                "description": "The name for the second SQL container"
            }
        }
    },
    "variables": {
        "accountName": "[toLower(parameters('accountName'))]",
        "consistencyPolicy": {
            "Eventual": {
                "defaultConsistencyLevel": "Eventual"
            },
            "ConsistentPrefix": {
                "defaultConsistencyLevel": "ConsistentPrefix"
            },
            "Session": {
                "defaultConsistencyLevel": "Session"
            },
            "BoundedStaleness": {
                "defaultConsistencyLevel": "BoundedStaleness",
                "maxStalenessPrefix": "[parameters('maxStalenessPrefix')]",
                "maxIntervalInSeconds": "[parameters('maxIntervalInSeconds')]"
            },
            "Strong": {
                "defaultConsistencyLevel": "Strong"
            }
        },
        "locations": 
        [ 
            {
                "locationName": "[parameters('primaryRegion')]",
                "failoverPriority": 0
            }, 
            {
                "locationName": "[parameters('secondaryRegion')]",
                "failoverPriority": 1
            }
        ]
    },
    "resources": 
    [
        {
            "type": "Microsoft.DocumentDB/databaseAccounts",
            "name": "[variables('accountName')]",
            "apiVersion": "2016-03-31",
            "location": "[parameters('location')]",
            "kind": "GlobalDocumentDB",
            "properties": {
                "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
                "locations": "[variables('locations')]",
                "databaseAccountOfferType": "Standard",
                "enableAutomaticFailover": "[parameters('automaticFailover')]",
                "enableMultipleWriteLocations": "[parameters('multipleWriteLocations')]"
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/apis/databases",
            "name": "[concat(variables('accountName'), '/sql/', parameters('databaseName'))]",
            "apiVersion": "2016-03-31",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/', variables('accountName'))]" ],
            "properties":{
                "resource":{
                    "id": "[parameters('databaseName')]"
                },
                "options": { "throughput": "[parameters('throughput')]" }
            }
        },
        {
            "type": "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers",
            "name": "[concat(variables('accountName'), '/sql/', parameters('databaseName'), '/', parameters('container1Name'))]",
            "apiVersion": "2016-03-31",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/apis/databases', variables('accountName'), 'sql', parameters('databaseName'))]" ],
            "properties":
            {
                "resource":{
                    "id":  "[parameters('container1Name')]",
                    "partitionKey": {
                        "paths": [
                        "/MyPartitionKey1"
                        ],
                        "kind": "Hash"
                    },
                    "indexingPolicy": {
                        "indexingMode": "consistent",
                        "includedPaths": [{
                                "path": "/*"
                            }
                        ],
                        "excludedPaths": [{
                                "path": "/MyPathToNotIndex/*"
                            }
                        ]
                    }
                }
            }
        },
        {
            "type": "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers",
            "name": "[concat(variables('accountName'), '/sql/', parameters('databaseName'), '/', parameters('container2Name'))]",
            "apiVersion": "2016-03-31",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/apis/databases', variables('accountName'), 'sql', parameters('databaseName'))]" ],
            "properties":
            {
                "resource":{
                    "id":  "[parameters('container2Name')]",
                    "partitionKey": {
                        "paths": [
                        "/MyPartitionKey2"
                        ],
                        "kind": "Hash"
                    },
                    "indexingPolicy": {
                        "indexingMode": "consistent",
                        "includedPaths": [{
                                "path": "/*"
                            }
                        ]
                    }
                }
            }
        }
    ]
}
```

### <a name="deploy-via-powershell"></a>通过 PowerShell 部署

若要部署资源管理器模板，请使用 PowerShell。

<!--Not Available on  **Copy** the script and select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

```powershell
Connect-AzAccount -Environment AzureChinaCloud

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. chinanorth2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. chinanorth2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. chinaeast2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$container1Name = Read-Host -Prompt "Enter the first container name"
$container2Name = Read-Host -Prompt "Enter the second container name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -container1Name $container1Name `
    -container2Name $container2Name

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2015-04-08" --ResourceGroupName $resourceGroupName).name
```

如果选择使用本地安装的 PowerShell 版本，则需[安装](https://docs.microsoft.com/powershell/azure/install-az-ps) Azure PowerShell 模块。 运行 `Get-Module -ListAvailable Az` 即可查找版本。 

<!--MOONCAKE: Not available on instead of using CloudShell-->

### <a name="deploy-via-azure-cli"></a>通过 Azure CLI 部署

使用 Azure 本地 CLI 部署资源管理器模板。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

<!--Not Available on **Copy** the script and select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

<!--MOONCAKE: parameter correct on --name $accountName-->

```azurecli
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. chinanorth2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. chinanorth2): ' primaryRegion
read -p 'Enter the secondary region (i.e. chinaeast2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first container name: ' container1Name
read -p 'Enter the second container name: ' container2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   container1Name=$container1Name container2Name=$container2Name

az cosmosdb show --resource-group $resourceGroupName --name $accountName --output tsv
```

`az cosmosdb show` 命令显示预配后的新建 Azure Cosmos 帐户。 

<!--Not Available on If you choose to use a locally installed version of Azure CLI instead of using CloudShell, see [Azure Command-Line Interface (CLI)](https://docs.azure.cn/cli/?view=azure-cli-latest) article.-->

## 更新数据库的吞吐量（RU/秒）<a name="database-ru-update"></a>

以下模板将更新数据库的吞吐量。 复制模板并按如下所示进行部署，或者访问 [Azure 快速入门库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-sql-database-ru-update/)，然后从 Azure 门户进行部署。 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "Cosmos account name"
            }
        },
        "databaseName": {
            "type": "string",
            "metadata": {
                "description": "Database name"
            }
        },
        "throughput": {
            "type": "int",
            "minValue": 400,
            "maxValue": 1000000,
            "metadata": {
                "description": "Updated throughput"
            }           
        }
    },
    "variables": {
        "accountName": "[toLower(parameters('accountName'))]"
    },
    "resources": 
    [
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/apis/databases/settings",
            "name": "[concat(variables('accountName'), '/sql/', parameters('databaseName'), '/throughput')]",
            "apiVersion": "2016-03-31",
            "properties": {
                "resource": {
                  "throughput": "[parameters('throughput')]"
                }
            }
        }
    ]
}
```

### <a name="deploy-database-template-via-powershell"></a>通过 PowerShell 部署数据库模板

若要部署资源管理器模板，请使用 PowerShell。

<!--Not Available on Cloud Shell-->
<!--Not Available To deploy the Resource Manager template using PowerShell, **Copy** the script and select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

```powershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$databaseName = Read-Host -Prompt "Enter the database name"
$throughput = Read-Host -Prompt "Enter new throughput for database"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-database-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -databaseName $databaseName `
    -throughput $throughput
```

### <a name="deploy-database-template-via-azure-cli"></a>通过 Azure CLI 部署数据库模板

使用 Azure CLI 部署资源管理器模板。

<!--Not Available on Cloud Shell-->
<!--Not Available onTo deploy the Resource Manager template using Azure CLI, select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

```azurecli
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

<a name="container-ru-update"></a>
## <a name="update-throughput-rus-on-a-container"></a>更新容器的吞吐量（RU/秒）

以下模板将更新容器的吞吐量。 复制模板并按如下所示进行部署，或者访问 [Azure 快速入门库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-sql-container-ru-update/)，然后从 Azure 门户进行部署。 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "Cosmos account name"
            }
        },
        "databaseName": {
            "type": "string",
            "metadata": {
                "description": "SQL database name"
            }
        },
        "containerName": {
            "type": "string",
            "metadata": {
                "description": "SQL container name"
            }
        },
        "throughput": {
            "type": "int",
            "minValue": 400,
            "maxValue": 1000000,
            "metadata": {
                "description": "Updated throughput amount"
            }           
        }
    },
    "variables": {
        "accountName": "[toLower(parameters('accountName'))]"
    },
    "resources": 
    [
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings",
            "name": "[concat(variables('accountName'), '/sql/', parameters('databaseName'), '/', parameters('containerName'), '/throughput')]",
            "apiVersion": "2016-03-31",
            "properties": {
                "resource": {
                  "throughput": "[parameters('throughput')]"
                }
            }
        }
    ]
}
```

### <a name="deploy-container-template-via-powershell"></a>通过 PowerShell 部署容器模板

若要部署资源管理器模板，请使用 PowerShell。

<!--Not Available on **Copy** the script and select **Try it** to open the Azure local Shell. To paste the script, right-click the shell, and then select **Paste**:-->

```powershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$databaseName = Read-Host -Prompt "Enter the database name"
$containerName = Read-Host -Prompt "Enter the container name"
$throughput = Read-Host -Prompt "Enter new throughput for container"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -databaseName $databaseName `
    -containerName $containerName `
    -throughput $throughput
```

### <a name="deploy-container-template-via-azure-cli"></a>通过 Azure CLI 部署容器模板

使用 Azure CLI 部署资源管理器模板。

<!--Not Available on  select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

```azurecli
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the container name: ' containerName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName containerName=$containerName throughput=$throughput
```

## <a name="next-steps"></a>后续步骤

下面是一些其他资源：

- [Azure 资源管理器文档](/azure-resource-manager/)
    
    <!--Not Available on  - [Azure Cosmos DB resource provider schema](https://docs.microsoft.com/zh-cn/azure/templates/microsoft.documentdb/allversions)-->
    
- [Azure Cosmos DB 快速入门模板](https://github.com/Azure/azure-quickstart-templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [排查常见的 Azure 资源管理器部署错误](../azure-resource-manager/resource-manager-common-deployment-errors.md)

<!--Update_Description: wording update -->
