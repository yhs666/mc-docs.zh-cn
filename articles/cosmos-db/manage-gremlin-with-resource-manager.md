---
title: Azure Cosmos DB Gremlin API 的 Azure 资源管理器模板
description: 使用 Azure 资源管理器模板创建和配置 Azure Cosmos DB Gremlin API。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 08/05/2019
ms.date: 09/09/2019
ms.author: v-yeche
ms.openlocfilehash: 59843f309873d2062cf48710ca923956171cbc72
ms.sourcegitcommit: 66192c23d7e5bf83d32311ae8fbb83e876e73534
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70254640"
---
# <a name="manage-azure-cosmos-db-gremlin-api-resources-using-azure-resource-manager-templates"></a>使用 Azure 资源管理器模板管理 Azure Cosmos DB Gremlin API 资源

## 创建 Azure Cosmos DB API for MongoDB 帐户、数据库和集合 <a name="create-resource"></a>

使用 Azure 资源管理器模板创建 Azure Cosmos DB 资源。 此模板将创建 Gremlin API 的 Azure Cosmos 帐户，所使用的两个图形在数据库级别共享 400 RU/秒的吞吐量。 复制模板并按如下所示进行部署，或者访问 [Azure 快速入门库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-gremlin/)，然后从 Azure 门户进行部署。 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

> [!NOTE]
> 帐户名称必须为小写且 < 31 个字符。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "defaultValue": "[uniqueString(resourceGroup().id)]",
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
        "primaryRegion": {
            "type": "string",
            "metadata": {
                "description": "The primary replica region for the Cosmos DB account."
            }
        },
        "secondaryRegion": {
            "type": "string",
            "metadata": {
                "description": "The secondary replica region for the Cosmos DB account."
            }
        },
        "defaultConsistencyLevel": {
            "type": "string",
            "defaultValue": "Session",
            "allowedValues": [
                "Eventual",
                "ConsistentPrefix",
                "Session",
                "BoundedStaleness",
                "Strong"
            ],
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
            "allowedValues": [
                true,
                false
            ],
            "metadata": {
                "description": "Enable multi-master to make all regions writable."
            }
        },
        "automaticFailover": {
            "type": "bool",
            "defaultValue": false,
            "allowedValues": [
                true,
                false
            ],
            "metadata": {
                "description": "Enable automatic failover for regions. Ignored when Multi-Master is enabled"
            }
        },
        "databaseName": {
            "type": "string",
            "defaultValue": "sampledb",
            "metadata": {
                "description": "The name for the Gremlin database"
            }
        },
        "throughput": {
            "type": "int",
            "defaultValue": 400,
            "minValue": 400,
            "maxValue": 1000000,
            "metadata": {
                "description": "Throughput shared across the Gremlin database"
            }
        },
        "graph1Name": {
            "type": "string",
            "defaultValue": "graph1",
            "metadata": {
                "description": "The name for the first Gremlin graph"
            }
        },
        "graph2Name": {
            "type": "string",
            "defaultValue": "graph2",
            "metadata": {
                "description": "The name for the second Gremlin graph"
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
        "locations": [
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
    "resources": [
        {
            "type": "Microsoft.DocumentDB/databaseAccounts",
            "name": "[variables('accountName')]",
            "apiVersion": "2016-03-31",
            "location": "[parameters('location')]",
            "tags": {},
            "kind": "GlobalDocumentDB",
            "properties": {
                "capabilities": [
                    {
                        "name": "EnableGremlin"
                    }
                ],
                "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
                "locations": "[variables('locations')]",
                "databaseAccountOfferType": "Standard",
                "enableAutomaticFailover": "[parameters('automaticFailover')]",
                "enableMultipleWriteLocations": "[parameters('multipleWriteLocations')]"
            }
        },
        {
            "type": "Microsoft.DocumentDB/databaseAccounts/apis/databases",
            "name": "[concat(variables('accountName'), '/gremlin/', parameters('databaseName'))]",
            "apiVersion": "2016-03-31",
            "dependsOn": [
                "[resourceId('Microsoft.DocumentDB/databaseAccounts/', variables('accountName'))]"
            ],
            "properties": {
                "resource": {
                    "id": "[parameters('databaseName')]"
                },
                "options": {
                    "throughput": "[parameters('throughput')]"
                }
            }
        },
        {
            "type": "Microsoft.DocumentDb/databaseAccounts/apis/databases/graphs",
            "name": "[concat(variables('accountName'), '/gremlin/', parameters('databaseName'), '/', parameters('graph1Name'))]",
            "apiVersion": "2016-03-31",
            "dependsOn": [
                "[resourceId('Microsoft.DocumentDB/databaseAccounts/apis/databases', variables('accountName'), 'gremlin', parameters('databaseName'))]"
            ],
            "properties": {
                "resource": {
                    "id": "[parameters('graph1Name')]",
                    "indexingPolicy": {
                        "indexingMode": "consistent",
                        "includedPaths": [
                            {
                                "path": "/*"
                            }
                        ],
                        "excludedPaths": [
                            {
                                "path": "/MyPathToNotIndex/*"
                            }
                        ]
                    },
                    "partitionKey": {
                        "paths": [
                            "/MyPartitionKey"
                        ],
                        "kind": "Hash"
                    }
                }
            }
        },
        {
            "type": "Microsoft.DocumentDb/databaseAccounts/apis/databases/graphs",
            "name": "[concat(variables('accountName'), '/gremlin/', parameters('databaseName'), '/', parameters('graph2Name'))]",
            "apiVersion": "2016-03-31",
            "dependsOn": [
                "[resourceId('Microsoft.DocumentDB/databaseAccounts/apis/databases', variables('accountName'), 'gremlin', parameters('databaseName'))]"
            ],
            "properties": {
                "resource": {
                    "id": "[parameters('graph2Name')]",
                    "indexingPolicy": {
                        "indexingMode": "consistent",
                        "includedPaths": [
                            {
                                "path": "/*"
                            }
                        ]
                    },
                    "partitionKey": {
                        "paths": [
                            "/MyPartitionKey"
                        ],
                        "kind": "Hash"
                    }
                }
            }
        }
    ]
}
```


## <a name="deploy-with-azure-cli"></a>使用 Azure CLI 进行部署

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
read -p 'Enter the first graph name: ' graph1Name
read -p 'Enter the second graph name: ' graph2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   graph1Name=$graph1Name graph2Name=$graph2Name

az cosmosdb show --resource-group $resourceGroupName --name $accountName --output tsv
```
<!--MOONCAKE: parameter correct on --name $accountName-->

`az cosmosdb show` 命令显示预配后的新建 Azure Cosmos 帐户。

<!--MOONCAKE: Not available on instead of using CloudShell-->

<a name="database-ru-update"></a>
## <a name="update-throughput-rus-on-a-database"></a>更新数据库的吞吐量（RU/秒） 

以下模板将更新数据库的吞吐量。 复制模板并按如下所示进行部署，或者访问 [Azure 快速入门库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-gremlin-database-ru-update/)，然后从 Azure 门户进行部署。 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

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
            "name": "[concat(variables('accountName'), '/gremlin/', parameters('databaseName'), '/throughput')]",
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

使用 Azure CLI 部署资源管理器模板。

<!--Not Available on  select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

```azurecli
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

<a name="graph-ru-update"></a>
## <a name="update-throughput-rus-on-a-graph"></a>更新图形的吞吐量（RU/秒） 

以下模板将更新图形的吞吐量。 复制模板并按如下所示进行部署，或者访问 [Azure 快速入门库](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-gremlin-graph-ru-update/)，然后从 Azure 门户进行部署。 还可以将模板下载到本地计算机，或者创建新模板并使用 `--template-file` 参数指定本地路径。

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
                "description": "Gremlin Database name"
            }
        },
        "graphName": {
            "type": "string",
            "metadata": {
                "description": "Gremlin Graph name"
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
            "type": "Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings",
            "name": "[concat(variables('accountName'), '/gremlin/', parameters('databaseName'), '/', parameters('graphName'), '/throughput')]",
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

### <a name="deploy-graph-template-via-azure-cli"></a>通过 Azure CLI 部署图形模板

使用 Azure CLI 部署资源管理器模板。

<!--Not Avaialble on Cloud Shell-->
<!--Not Avaialble on To deploy the Resource Manager template using Azure CLI, select **Try it** to open the Azure Cloud shell. To paste the script, right-click the shell, and then select **Paste**:-->

```azurecli
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the graph name: ' graphName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin-graph-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName graphName=$graphName throughput=$throughput
```

## <a name="next-steps"></a>后续步骤

下面是一些其他资源：

- [Azure 资源管理器文档](/azure-resource-manager/)

    <!--Not Available on - [Azure Cosmos DB resource provider schema](https://docs.microsoft.com/azure/templates/microsoft.documentdb/allversions)-->
    
- [Azure Cosmos DB 快速入门模板](https://github.com/Azure/azure-quickstart-templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [排查常见的 Azure 资源管理器部署错误](../azure-resource-manager/resource-manager-common-deployment-errors.md)

<!--Update_Description: wording update-->