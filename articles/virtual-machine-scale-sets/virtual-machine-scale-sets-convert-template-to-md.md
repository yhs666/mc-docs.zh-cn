---
title: 将 Azure Resource Manager 规模集模板转换为使用托管磁盘 |Azure
description: 将规模集模板转换为托管磁盘规模集模板
keywords: 虚拟机规模集
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager

ms.assetid: bc8c377a-8c3f-45b8-8b2d-acc2d6d0b1e8
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/25/2017
wacn.date: 03/03/2017
ms.author: negat
---

# 将规模集模板转换为托管磁盘规模集模板

使用 Resource Manager 模板创建不使用托管的磁盘的规模集的客户可能希望改为使用托管的磁盘。本文演示如何实现此操作，并以 [Azure 快速入门模板](https://github.com/Azure/azure-quickstart-templates)中的拉取请求和示例 Resource Manager 模板的社区主导存储库为例。可从以下网址查看完整的拉取请求：[https://github.com/Azure/azure-quickstart-templates/pull/2998](https://github.com/Azure/azure-quickstart-templates/pull/2998)，相关的差异比较部分及说明如下所示：

## 托管 OS 磁盘

在下面的差异比较中，可以看到已删除与存储帐户和磁盘属性相关的多个变量。不再需要存储帐户类型（Standard\_LRS 是默认值），但是如果需要，我们仍可以指定它。托管磁盘仅支持 Standard\_LRS 和 Premium\_LRS。旧模板中使用新存储帐户的后缀、唯一字符串数组和 sa 计数来生成存储帐户名称。新模板不再需要这些变量，因为托管磁盘代表客户自动创建存储帐户。同样，也不再需要 vhd 容器名称和 os 磁盘名称，因为托管磁盘自动命名基础存储 blob 容器和磁盘。

```diff
   "variables": {
-    "storageAccountType": "Standard_LRS",
     "namingInfix": "[toLower(substring(concat(parameters('vmssName'), uniqueString(resourceGroup().id)), 0, 9))]",
     "longNamingInfix": "[toLower(parameters('vmssName'))]",
-    "newStorageAccountSuffix": "[concat(variables('namingInfix'), 'sa')]",
-    "uniqueStringArray": [
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '0')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '1')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '2')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '3')))]",
-      "[concat(uniqueString(concat(resourceGroup().id, variables('newStorageAccountSuffix'), '4')))]"
-    ],
-    "saCount": "[length(variables('uniqueStringArray'))]",
-    "vhdContainerName": "[concat(variables('namingInfix'), 'vhd')]",
-    "osDiskName": "[concat(variables('namingInfix'), 'osdisk')]",
     "addressPrefix": "10.0.0.0/16",
     "subnetPrefix": "10.0.0.0/24",
     "virtualNetworkName": "[concat(variables('namingInfix'), 'vnet')]",
```

在下面的差异比较中，可以看到已将计算 api 版本更新为 2016-04-30-preview，这是规模集支持的托管磁盘所需的最早版本。请注意，如果需要，我们仍可以使用旧的语法在新的 api 版本中使用非托管磁盘。换而言之，如果只更新计算 api 版本，而不更改任何其他内容，仍可以和以前一样继续使用模板。

```diff
@@ -86,7 +74,7 @@
       "version": "latest"
     },
     "imageReference": "[variables('osType')]",
-    "computeApiVersion": "2016-03-30",
+    "computeApiVersion": "2016-04-30-preview",
     "networkApiVersion": "2016-03-30",
     "storageApiVersion": "2015-06-15"
   },
```

在下面的差异比较中，可以看到从资源数组中完全删除存储帐户资源。我们不再需要这些资源，因为托管磁盘代表我们自动创建了这些资源。

```diff
@@ -113,19 +101,6 @@
       }
     },
-    {
-      "type": "Microsoft.Storage/storageAccounts",
-      "name": "[concat(variables('uniqueStringArray')[copyIndex()], variables('newStorageAccountSuffix'))]",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "[variables('storageApiVersion')]",
-      "copy": {
-        "name": "storageLoop",
-        "count": "[variables('saCount')]"
-      },
-      "properties": {
-        "accountType": "[variables('storageAccountType')]"
-      }
-    },
     {
       "type": "Microsoft.Network/publicIPAddresses",
       "name": "[variables('publicIPAddressName')]",
       "location": "[resourceGroup().location]",
```

在下面的差异比较中，可以看到删除了从规模集到循环（创建存储帐户）引用的 depends on 子句。在旧的模板中，这可以确保在开始创建规模集之前已创建存储帐户，但是托管磁盘不再需要该子句。我们还删除了 vhd 容器属性和 os 磁盘名称属性，因为托管磁盘已在后台自动处理了这些属性。如果需要并想要使用高级 OS 磁盘，可在“osDisk”配置中添加 `"managedDisk": { "storageAccountType": "Premium_LRS" }`。只有在 VM sku 中包含大写或小写的“s”的 VM 才可以使用高级磁盘。

```diff
@@ -183,7 +158,6 @@
       "location": "[resourceGroup().location]",
       "apiVersion": "[variables('computeApiVersion')]",
       "dependsOn": [
-        "storageLoop",
         "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
         "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
       ],
@@ -200,16 +174,8 @@
         "virtualMachineProfile": {
           "storageProfile": {
             "osDisk": {
-              "vhdContainers": [
-                "[concat('https://', variables('uniqueStringArray')[0], variables('newStorageAccountSuffix'), '.blob.core.chinacloudapi.cn/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[1], variables('newStorageAccountSuffix'), '.blob.core.chinacloudapi.cn/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[2], variables('newStorageAccountSuffix'), '.blob.core.chinacloudapi.cn/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[3], variables('newStorageAccountSuffix'), '.blob.core.chinacloudapi.cn/', variables('vhdContainerName'))]",
-                "[concat('https://', variables('uniqueStringArray')[4], variables('newStorageAccountSuffix'), '.blob.core.chinacloudapi.cn/', variables('vhdContainerName'))]"
-              ],
-              "name": "[variables('osDiskName')]",
             },
             "imageReference": "[variables('imageReference')]"
           },
```

规模集配置中不存在是使用托管磁盘还是非托管磁盘的显式属性。根据存储配置文件中显示的属性，规模集知道使用哪种磁盘。因此，务必在修改模板时确保规模集的存储配置文件中具有正确的属性。

## 数据磁盘数

进行上述更改后，规模集对 OS 磁盘使用托管磁盘，但是如果要使用数据磁盘呢？ 若要添加数据磁盘，请在与“osDisk”相同级别的“storageProfile”下面添加“dataDisks”属性。该属性的值为一个 JSON 对象列表，其中每个对象都具有属性“lun”（VM 上每个数据磁盘的 lun 必须是唯一的）、“createOption”（当前唯一支持的选项为“empty”）和“diskSizeGB”（以千兆字节为单位的磁盘的大小，该值必须大于 0 且小于 1024），如以下示例所示：

```
"dataDisks": [
  {
    "lun": "1",
    "createOption": "empty",
    "diskSizeGB": "1023"
  }
]
```

如果在此数组中指定 `n` 个磁盘，那么规模集中的每个 VM 都将获取 `n` 个数据磁盘。但是，请注意，这些数据磁盘都是原始设备。它们未经格式化。客户需要在使用它们之前对这些磁盘执行附加、分区和格式化操作。此外，还可以在每个数据磁盘对象中指定 `"managedDisk": { "storageAccountType": "Premium_LRS" }` 以指定高级数据磁盘。只有在 VM sku 中包含大写或小写的“s”的 VM 才可以使用高级磁盘。

若要了解有关在规模集中使用数据磁盘的详细信息，请参阅[本文](./virtual-machine-scale-sets-attached-disks.md)。

## 后续步骤
有关使用规模集的示例 Resource Manager 模板，请在 [Azure 快速入门模板 github 存储库](https://github.com/Azure/azure-quickstart-templates)中搜索“vmss”。

有关一般信息，请参阅[规模集的主要登陆页](https://www.azure.cn/home/features/virtual-machine-scale-sets/)。

<!---HONumber=Mooncake_0227_2017-->