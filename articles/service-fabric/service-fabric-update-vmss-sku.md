---
title: 将 PrimaryNodeType 的 SKU 升级/迁移到更高的 SKU 的过程 | Azure
description: 了解如何使用 PowerShell 命令和 CLI 命令将 PrimaryNodeType 的 SKU 升级/迁移到更高的 SKU。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 05/08/2018
ms.date: 05/28/2018
ms.author: v-yeche
ms.openlocfilehash: a64f83ede166f105f8b4d2d321d45588380c9e68
ms.sourcegitcommit: e50f668257c023ca59d7a1df9f1fe02a51757719
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/26/2018
ms.locfileid: "34554688"
---
# <a name="upgrademigrate-the-sku-for-primary-node-type-to-higher-sku"></a>将主节点类型的 SKU 升级/迁移到更高的 SKU

本文介绍了如何使用 Azure PowerShell 将 Service Fabric 群集的主节点类型的 SKU 升级/迁移到更高的 SKU

## <a name="add-a-new-virtual-machine-scale-set"></a>添加新的虚拟机规模集 

部署新的虚拟机规模集和负载均衡器。 新虚拟机规模集的 Service Fabric 扩展配置（尤其是节点类型）应与你尝试升级的旧规模集的扩展配置相同。 请在 SF 资源管理器中验证新节点是否可用。 

### <a name="azure-powershell"></a>Azure PowerShell
以下示例通过 Azure PowerShell 使用名为 *myResourceGroup* 的资源组部署更新后的资源管理器模板 *template.json*：

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName myResourceGroup -TemplateFile \template\template.json 
```

请参阅以下示例修改 json 模板，以在现有群集中添加具有主节点类型的新虚拟机规模集资源

```json
        {
            "apiVersion": "[variables('vmssApiVersion')]",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[parameters('vmNodeType2Name')]",
            "location": "[parameters('computeLocation')]",
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
                "[concat('Microsoft.Network/loadBalancers/', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType2Name')))]",
                "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]",
                "[concat('Microsoft.Storage/storageAccounts/', parameters('applicationDiagnosticsStorageAccountName'))]"
            ],
            "properties": {
                "overprovision": "[parameters('overProvision')]",
                "upgradePolicy": {
                    "mode": "Automatic"
                },
                "virtualMachineProfile": {
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "[concat(parameters('vmNodeType2Name'),'_ServiceFabricNode')]",
                                "properties": {
                                    "type": "ServiceFabricNode",
                                    "autoUpgradeMinorVersion": true,
                                    "protectedSettings": {
                                        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                                        "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                                    },
                                    "publisher": "Microsoft.Azure.ServiceFabric",
                                    "settings": {
                                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                                        "dataPath": "D:\\\\SvcFab",
                                        "durabilityLevel": "Bronze",
                                        "enableParallelJobs": true,
                                        "nicPrefixOverride": "[parameters('subnet0Prefix')]",
                                        "certificate": {
                                            "thumbprint": "[parameters('certificateThumbprint')]",
                                            "x509StoreName": "[parameters('certificateStoreValue')]"
                                        }
                                    },
                                    "typeHandlerVersion": "1.0"
                                }
                            },
                            {
                                "name": "[concat('VMDiagnosticsVmExt','_vmNodeType2Name')]",
                                "properties": {
                                    "type": "IaaSDiagnostics",
                                    "autoUpgradeMinorVersion": true,
                                    "protectedSettings": {
                                        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
                                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
                                        "storageAccountEndPoint": "https://core.chinacloudapi.cn/"
                                    },
                                    "publisher": "Microsoft.Azure.Diagnostics",
                                    "settings": {
                                        "WadCfg": {
                                            "DiagnosticMonitorConfiguration": {
                                                "overallQuotaInMB": "50000",
                                                "EtwProviders": {
                                                    "EtwEventSourceProviderConfiguration": [
                                                        {
                                                            "provider": "Microsoft-ServiceFabric-Actors",
                                                            "scheduledTransferKeywordFilter": "1",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricReliableActorEventTable"
                                                            }
                                                        },
                                                        {
                                                            "provider": "Microsoft-ServiceFabric-Services",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricReliableServiceEventTable"
                                                            }
                                                        }
                                                    ],
                                                    "EtwManifestProviderConfiguration": [
                                                        {
                                                            "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                                                            "scheduledTransferLogLevelFilter": "Information",
                                                            "scheduledTransferKeywordFilter": "4611686018427387904",
                                                            "scheduledTransferPeriod": "PT5M",
                                                            "DefaultEvents": {
                                                                "eventDestination": "ServiceFabricSystemEventTable"
                                                            }
                                                        }
                                                    ]
                                                }
                                            }
                                        },
                                        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
                                    },
                                    "typeHandlerVersion": "1.5"
                                }
                            }
                        ]
                    },
                    "networkProfile": {
                        "networkInterfaceConfigurations": [
                            {
                                "name": "[concat(parameters('nicName'), '-2')]",
                                "properties": {
                                    "ipConfigurations": [
                                        {
                                            "name": "[concat(parameters('nicName'),'-',2)]",
                                            "properties": {
                                                "loadBalancerBackendAddressPools": [
                                                    {
                                                        "id": "[variables('lbPoolID2')]"
                                                    }
                                                ],
                                                "loadBalancerInboundNatPools": [
                                                    {
                                                        "id": "[variables('lbNatPoolID2')]"
                                                    }
                                                ],
                                                "subnet": {
                                                    "id": "[variables('subnet0Ref')]"
                                                }
                                            }
                                        }
                                    ],
                                    "primary": true
                                }
                            }
                        ]
                    },
                    "osProfile": {
                        "adminPassword": "[parameters('adminPassword')]",
                        "adminUsername": "[parameters('adminUsername')]",
                        "computernamePrefix": "[parameters('vmNodeType2Name')]",
                        "secrets": [
                            {
                                "sourceVault": {
                                    "id": "[parameters('sourceVaultValue')]"
                                },
                                "vaultCertificates": [
                                    {
                                        "certificateStore": "[parameters('certificateStoreValue')]",
                                        "certificateUrl": "[parameters('certificateUrlValue')]"
                                    }
                                ]
                            }
                        ]
                    },
                    "storageProfile": {
                        "imageReference": {
                            "publisher": "[parameters('vmImagePublisher')]",
                            "offer": "[parameters('vmImageOffer')]",
                            "sku": "[parameters('vmImageSku')]",
                            "version": "[parameters('vmImageVersion')]"
                        },
                        "osDisk": {
                            "caching": "ReadOnly",
                            "createOption": "FromImage",
                            "managedDisk": {
                                "storageAccountType": "[parameters('storageAccountType')]"
                            }
                        }
                    }
                }
            },
            "sku": {
                "name": "[parameters('vmNodeType2Size')]",
                "capacity": "[parameters('nt2InstanceCount')]",
                "tier": "Standard"
            },
            "tags": {
                "resourceType": "Service Fabric",
                "clusterName": "[parameters('clusterName')]"
            }
        },
```

## <a name="remove-old-virtual-machine-scale-set"></a>删除旧的虚拟机规模集

禁用旧虚拟机规模集的 VM 实例以便删除节点。 此操作可能需要较长时间才能完成。 可以一次全部禁用，它们会排队。 请等待直至所有节点都被禁用。 

### <a name="azure-powershell"></a>Azure PowerShell
以下示例使用 Azure Service Fabric PowerShell 禁用名为 *NTvm1* 的旧虚拟机规模集中名为 *NTvm1_0* 的节点实例：

```powershell
Disable-ServiceFabricNode -NodeName NTvm1_0 -Intent RemoveNode 
```

删除整个规模集。 请等待直至规模集预配状态为成功 

### <a name="azure-powershell"></a>Azure PowerShell
以下示例使用 Azure PowerShell 从名为 *myResourceGroup* 的资源组中删除名为 *NTvm1* 的整个规模集：

```powershell
Remove-AzureRmVmss -ResourceGroupName myResourceGroup -VMScaleSetName NTvm1
```

## <a name="remove-load-balancer-related-to-old-scale-set"></a>删除与旧规模集相关的负载均衡器

删除与旧规模集相关的负载均衡器。 此步骤将导致群集出现短暂的故障时间

### <a name="azure-powershell"></a>Azure PowerShell
以下示例使用 Azure PowerShell 从名为 *myResourceGroup* 的资源组中删除与旧规模集相关的名为 *LB-myCluster-NTvm1* 的负载均衡器：

```powershell
Remove-AzureRmLoadBalancer -Name LB-myCluster-NTvm1 -ResourceGroupName myResourceGroup
```

## <a name="remove-public-ip-related-to-old-scale-set"></a>删除与旧规模集相关的公用 IP

将与旧规模集相关的公用 IP 地址的 DNS 设置存储到变量中，然后删除该公用 IP 地址

### <a name="azure-powershell"></a>Azure PowerShell
以下示例使用 Azure PowerShell 将名为 *LBIP-myCluster-NTvm1* 的公用 IP 地址的 DNS 设置存储到变量中，然后删除该 IP 地址：

```powershell
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1  -ResourceGroupName myResourceGroup
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn
Remove-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1 -ResourceGroupName myResourceGroup
```

## <a name="update-public-ip-address-related-to-new-scale-set"></a>更新与新规模集相关的公用 IP 地址

使用与旧规模集相关的公用 IP 地址的 DNS 设置更新与新规模集相关的公用 IP 地址的 DNS 设置

### <a name="azure-powershell"></a>Azure PowerShell
以下示例使用 Azure PowerShell 将名为 *LBIP-myCluster-NTvm3* 的公用 IP 地址的 DNS 设置更新为上一步中存储在变量中的 DNS 设置：

```powershell
$PublicIP = Get-AzureRmPublicIpAddress -Name LBIP-myCluster-NTvm1  -ResourceGroupName myResourceGroup
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP 
```

## <a name="remove-knowledge-of-service-fabric-node-from-fm"></a>从 FM 中删除 Service Fabric 节点的信息

通知 Service Fabric，已关闭的节点已从群集中删除。 （对旧虚拟机规模集的所有 VM 实例运行此命令）（如果旧虚拟机规模集的耐久性级别为“白银”或“黄金”，则可能不需要执行此步骤。 因为系统会自动执行该步骤。）

### <a name="azure-powershell"></a>Azure PowerShell
以下示例使用 Azure Service Fabric PowerShell 来通知 Service Fabric 名为 *NTvm1_0* 的节点已删除：

```powershell
Remove-ServiceFabricNodeState -NodeName NTvm1_0
```
<!-- Update_Description: new articles on service fabric update vmss sku -->
<!--ms.date: 05/28/2018-->