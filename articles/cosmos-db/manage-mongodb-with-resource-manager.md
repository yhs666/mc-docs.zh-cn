---
title: Azure Cosmos DB API for MongoDB 的 Azure 资源管理器模板
description: 使用 Azure 资源管理器模板创建和配置 Azure Cosmos DB API for MongoDB。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 05/20/2019
ms.date: 06/17/2019
ms.author: v-yeche
ms.openlocfilehash: e636afaa91f6196ad5a1011a803f7dffd79d91c5
ms.sourcegitcommit: 48a45ba95a6d1c15110191409deb0e7aac4bd88b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2019
ms.locfileid: "68293431"
---
<!--Verify successfully-->
# <a name="manage-azure-cosmos-db-mongodb-api-resources-using-azure-resource-manager-templates"></a>使用 Azure 资源管理器模板管理 Azure Cosmos DB MongoDB API 资源

## 创建 Azure Cosmos DB API for MongoDB 帐户、数据库和集合 <a id="create-resource"></a>

使用 Azure 资源管理器模板创建 Azure Cosmos DB 资源。 此模板将创建 MongoDB API 的 Azure Cosmos 帐户，所使用的两个集合在数据库级别共享 400 RU/秒的吞吐量。 复制模板并按如下所示进行部署，或者访问 [Azure 快速入门库](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb/)，然后从 Azure 门户进行部署。 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "defaultValue": "[concat('mongodb-', uniqueString(resourceGroup().id))]",
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
            "defaultValue": 100000,
            "minValue": 10,
            "maxValue": 2147483647,
            "metadata": {
                "description": "Max stale requests. Required for BoundedStaleness. Valid ranges, Single Region: 10 to 1000000. Multi Region: 100000 to 1000000."
            }
        },
        "maxIntervalInSeconds": {
            "type": "int",
            "defaultValue": 300,
            "minValue": 5,
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
        "databaseName": {
            "type": "string",
            "metadata": {
                "description": "The name for the Mongo DB database"
            }
        },
        "throughput": {
            "type": "int",
            "defaultValue": 400,
            "minValue": 400,
            "maxValue": 1000000,
            "metadata": {
                "description": "The throughput for the Mongo DB database"
            }           
        },
        "collection1Name": {
            "type": "string",
            "metadata": {
                "description": "The name for the first Mongo DB collection"
            }
        },
        "collection2Name": {
            "type": "string",
            "metadata": {
                "description": "The name for the second Mongo DB collection"
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
            "kind": "MongoDB",
            "properties": {
                "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
                "locations": "[variables('locations')]",
                "databaseAccountOfferType": "Standard",
                "enableMultipleWriteLocations": "[parameters('multipleWriteLocations')]"
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/apis/databases",
            "name": "[concat(variables('accountName'), '/mongodb/', parameters('databaseName'))]",
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
            "type": "Microsoft.DocumentDb/databaseAccounts/apis/databases/collections",
            "name": "[concat(variables('accountName'), '/mongodb/', parameters('databaseName'), '/', parameters('collection1Name'))]",
            "apiVersion": "2016-03-31",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/apis/databases', variables('accountName'), 'mongodb', parameters('databaseName'))]" ],
            "properties":
            {
                "resource":{
                    "id":  "[parameters('collection1Name')]",
                    "shardKey": { "user_id": "Hash" },
                    "indexes": [
                        {
                            "key": { "keys":["user_id", "user_address"] },
                            "options": { "unique": "true" }
                        },
                        {
                            "key": { "keys":["_ts"] },
                            "options": { "expireAfterSeconds": "2629746" }
                        }
                    ],
                    "options": {
                        "If-Match": "<ETag>"
                    }
                }
            }
        },
        {
            "type": "Microsoft.DocumentDb/databaseAccounts/apis/databases/collections",
            "name": "[concat(variables('accountName'), '/mongodb/', parameters('databaseName'), '/', parameters('collection2Name'))]",
            "apiVersion": "2016-03-31",
            "dependsOn": [ "[resourceId('Microsoft.DocumentDB/databaseAccounts/apis/databases', variables('accountName'), 'mongodb', parameters('databaseName'))]" ],
            "properties":
            {
                "resource":{
                    "id":  "[parameters('collection2Name')]",
                    "shardKey": { "company_id": "Hash" },
                    "indexes": [
                        {
                            "key": { "keys":["company_id", "company_address"] },
                            "options": { "unique": "true" }
                        },
                        {
                            "key": { "keys":["_ts"] },
                            "options": { "expireAfterSeconds": "2629746" }
                        }
                    ],
                    "options": {
                        "If-Match": "<ETag>"
                    }
                }
            }
        }
    ]
}
```

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
read -p 'Enter the first collection name: ' collection1Name
read -p 'Enter the second collection name: ' collection2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb/azuredeploy.json \
  --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion \
  databaseName=$databaseName collection1Name=$collection1Name collection2Name=$collection2Name

az cosmosdb show --resource-group $resourceGroupName --name $accountName --output tsv
```

`az cosmosdb show` 命令显示预配后的新建 Azure Cosmos 帐户。 

<!--Not Available on If you choose to use a locally installed version of Azure CLI instead of using CloudShell, see [Azure Command-Line Interface (CLI)](/cli/azure/) article.-->

## 更新数据库的吞吐量（RU/秒）<a id="database-ru-update"></a>

以下模板将更新数据库的吞吐量。 复制模板并按如下所示进行部署，或者访问 [Azure 快速入门库](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb-database-ru-update/)，然后从 Azure 门户进行部署。 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

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
            "name": "[concat(variables('accountName'), '/mongodb/', parameters('databaseName'), '/throughput')]",
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

### <a name="deploy-database-template-via-azure-cli"></a>通过 Azure CLI 部署数据库模板

<!--Not Available on Cloud Shell-->
<!--Not Available on select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

使用 Azure CLI 部署资源管理器模板

```azurecli
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

<a id="collection-ru-update"></a>
## <a name="update-throughput-rus-on-a-collection"></a>更新集合的吞吐量（RU/秒） 

以下模板将更新集合的吞吐量。 复制模板并按如下所示进行部署，或者访问 [Azure 快速入门库](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb-collection-ru-update/)，然后从 Azure 门户进行部署。 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

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
        "collectionName": {
            "type": "string",
            "metadata": {
                "description": "Collection name"
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
            "type": "Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings",
            "name": "[concat(variables('accountName'), '/mongodb/', parameters('databaseName'), '/', parameters('collectionName'), '/throughput')]",
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

### <a name="deploy-collection-template-via-azure-cli"></a>通过 Azure CLI 部署集合模板

使用 Azure CLI 部署资源管理器模板。

<!--Not Available on  select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

```azurecli
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the collection name: ' collectionName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb-collection-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName collectionName=$collectionName throughput=$throughput
```

## <a name="next-steps"></a>后续步骤

下面是一些其他资源：

- [Azure 资源管理器文档](/azure-resource-manager/)
- [Azure Cosmos DB 资源提供程序架构](https://docs.microsoft.com/zh-cn/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB 快速入门模板](https://github.com/Azure/azure-quickstart-templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [排查常见的 Azure 资源管理器部署错误](../azure-resource-manager/resource-manager-common-deployment-errors.md)

<!--Update_Description: wording upate -->
