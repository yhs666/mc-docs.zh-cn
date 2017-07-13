---
title: "使用模板创建 Azure 事件中心命名空间并启用存档 | Azure"
description: "使用 Azure Resource Manager 模板创建包含一个事件中心的 Azure 事件中心命名空间并启用存档"
services: event-hubs
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: 
ms.assetid: 8bdda6a2-5ff1-45e3-b696-c553768f1090
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
origin.date: 03/07/2017
ms.date: 07/03/2017
ms.author: v-yeche
ms.openlocfilehash: b75042f64f0b79da8f544a0710e484525c0ef5e5
ms.sourcegitcommit: cc3f528827a8acd109ba793eee023b8c6b2b75e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
# 使用 Azure Resource Manager 模板创建包含事件中心的事件中心命名空间并启用存档
<a id="create-an-event-hubs-namespace-with-an-event-hub-and-enable-archive-using-an-azure-resource-manager-template" class="xliff"></a>
本文介绍如何使用 Azure Resource Manager 模板创建包含一个事件中心实例的事件中心类型的命名空间，并在事件中心上启用存档功能。 本文介绍如何定义要部署的资源以及如何定义执行部署时指定的参数。 可将此模板用于自己的部署，或自定义此模板以满足要求

有关创建模板的详细信息，请参阅 [创作 Azure Resource Manager 模板][Authoring Azure Resource Manager templates]。

<!-- Not Available for [Azure Resources Naming Conventions][Azure Resources Naming Conventions] -->
有关完整的模板，请参阅 GitHub 上的 [事件中心和启用存档模板][Event Hub and enable Archive template] 。

> [!NOTE]
> 若要检查最新模板，请访问 [Azure 快速启动模板][Azure Quickstart Templates] 库并搜索事件中心。
> 
> 

## 你将部署什么内容？
<a id="what-will-you-deploy" class="xliff"></a>
借助此模板，可以部署包含事件中心的事件中心命名空间并启用事件中心存档。

[事件中心](event-hubs-what-is-event-hubs.md)是一种事件处理服务，用于向 Azure 提供大规模的事件与遥测数据入口，并且具有较低的延迟和较高的可靠性。 使用事件中心存档，可以按照所选指定时间或大小间隔将事件中心中的流数据自动传送到所选的 Azure Blob 存储。

若要自动运行部署，请单击以下按钮：

[![部署到 Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## Parameters
<a id="parameters" class="xliff"></a>

使用 Azure Resource Manager，可以定义在部署模板时想要指定的值的参数。 该模板具有一个名为 `Parameters` 的部分，其中包含所有参数值。 你应该为随着要部署的项目或要部署到的环境而变化的值定义参数。 不要为永远保持不变的值定义参数。 每个参数值可在模板中用来定义所部署的资源。

模板定义以下参数。

### eventHubNamespaceName
<a id="eventhubnamespacename" class="xliff"></a>

要创建的事件中心命名空间的名称。

```json
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### eventHubName
<a id="eventhubname" class="xliff"></a>

在事件中心命名空间中创建的事件中心的名称。

```json
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the event hub"
    }
}
```

### messageRetentionInDays
<a id="messageretentionindays" class="xliff"></a>

要在事件中心内保留消息的天数。 

```json
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in event hub"
     }
 }
```

### partitionCount
<a id="partitioncount" class="xliff"></a>

要在事件中心内创建的分区数。

```json
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### archiveEnabled
<a id="archiveenabled" class="xliff"></a>
在事件中心上启用存档。

```json
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your event hub"
    }
 }
```

### archiveEncodingFormat
<a id="archiveencodingformat" class="xliff"></a>

指定用于序列化事件数据的编码格式。

```json
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### archiveTime
<a id="archivetime" class="xliff"></a>
事件中心存档开始将数据存档在 Azure Blob 存储中的时间间隔。

```json
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### archiveSize
<a id="archivesize" class="xliff"></a>
存档开始将数据存档在 Azure blob 存储中的大小间隔。

```json
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### destinationStorageAccountResourceId
<a id="destinationstorageaccountresourceid" class="xliff"></a>
存档需要 Azure 存储帐户资源 ID，以启用到所需存储帐户的存档。

```json
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### blobContainerName
<a id="blobcontainername" class="xliff"></a>
用于存档事件数据的 blob 容器。

```json
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage container that you want the blobs archived in"
    }
}
```

### apiVersion
<a id="apiversion" class="xliff"></a>

模板的 API 版本。

```json
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## 要部署的资源
<a id="resources-to-deploy" class="xliff"></a>
创建包含一个事件中心的 **EventHubs** 类型的命名空间并启用存档。

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }

               }

            }
         ]
      }
   ]
```

## 运行部署的命令
<a id="commands-to-run-deployment" class="xliff"></a>

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## PowerShell
<a id="powershell" class="xliff"></a>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## Azure CLI
<a id="azure-cli" class="xliff"></a>

```cli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

## 后续步骤
<a id="next-steps" class="xliff"></a>
访问以下链接可以了解有关事件中心的详细信息：

* [事件中心概述](event-hubs-what-is-event-hubs.md)
* [创建事件中心](event-hubs-create.md)
* [事件中心常见问题](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://github.com/Azure/azure-quickstart-templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
<!-- Not Available [Azure Resources Naming Conventions]: ../guidance-naming-conventions.md-->
[Event hub and enable Archive template]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive