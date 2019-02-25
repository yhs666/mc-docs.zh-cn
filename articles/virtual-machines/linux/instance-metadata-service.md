---
title: Azure 实例元数据服务 | Azure
description: 一个 RESTful 接口，用于获取有关 Linux VM 计算、网络和即将发生的维护事件的信息。
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
origin.date: 10/10/2017
ms.date: 02/18/2019
ms.author: v-yeche
ms.openlocfilehash: 4831e2332099d683d0139567eccb4718bd3aa878
ms.sourcegitcommit: dd6cee8483c02c18fd46417d5d3bcc2cfdaf7db4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/22/2019
ms.locfileid: "56665936"
---
# <a name="azure-instance-metadata-service"></a>Azure 实例元数据服务

Azure 实例元数据服务提供有关可用于管理和配置虚拟机的正在运行的虚拟机实例的信息。
这包括 SKU、网络配置和即将发生的维护事件等信息。 若要详细了解可用信息类型，请参阅[元数据类别](#instance-metadata-data-categories)。

Azure 的实例元数据服务是一个 REST 终结点，可供通过 [Azure 资源管理器](https://docs.microsoft.com/rest/api/resources/)创建的 IaaS VM 使用。 该终结点位于已知不可路由的 IP 地址 (`169.254.169.254`)，该地址只能从 VM 中访问。

> [!IMPORTANT]
> 此服务已在 Azure 区域公开上市。  它会定期接收更新，发布有关虚拟机实例的新信息。 此页反映了最新可用的[数据类别](#instance-metadata-data-categories)。

## <a name="service-availability"></a>服务可用性
此服务在所有 Azure 区域中提供有可用的正式版。 并非所有 API 版本在所有 Azure 区域中可用。

区域                                        | 可用性？                                 | 支持的版本
-----------------------------------------------|-----------------------------------------------|-----------------
[全球所有公开上市的 Azure 区域](https://azure.microsoft.com/regions/)     | 正式版   | 2017-04-02、2017-08-01、2017-12-01、2018-02-01、2018-04-02
[Azure 美国政府版](https://azure.microsoft.com/overview/clouds/government/)              | 正式版 | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01
[Azure 中国](https://www.azure.cn/)                                                           | 正式版 | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01
[Azure 德国](https://azure.microsoft.com/overview/clouds/germany/)                    | 正式版 | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01

<!-- Notice: Correct [All Generally Available Global Azure Regions](https://azure.microsoft.com/regions/) -->
<!-- Notice : [Azure Government] to [Azure US Government] -->

当有服务更新和/或有可用的新支持版本时，此表将更新

若要试用实例元数据服务，请在上述区域中从 [Azure 资源管理器](https://docs.microsoft.com/rest/api/resources/)或 [Azure 门户](http://portal.azure.cn)创建一个 VM，并按照以下示例操作。

## <a name="usage"></a>使用情况

### <a name="versioning"></a>版本控制
已对实例元数据服务进行了版本控制。 版本是必需的，全局 Azure 上的当前版本为 `2018-04-02`。 当前支持的版本为（2017-04-02、2017-08-01、2017-12-01、2018-02-01、2018-04-02）

> [!NOTE] 
> 支持的计划事件的前一预览版 {latest} 发布为 api-version。 此格式不再受支持，并且将在未来弃用。

在添加更新的版本时，早期版本仍可供访问以保持兼容性（如果脚本依赖于特定的数据格式）。 但是，在服务正式发布之后，先前的预览版本（2017-03-01）可能不再可用。

### <a name="using-headers"></a>使用标头
查询实例元数据服务时，必须提供标头 `Metadata: true` 以确保不会意外重定向请求。

### <a name="retrieving-metadata"></a>检索元数据

实例元数据可用于运行使用 [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) 创建/管理的 VM。 使用以下请求访问虚拟机实例的所有数据类别：

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01"
```

> [!NOTE] 
> 所有实例元数据查询都区分大小写。

### <a name="data-output"></a>数据输出
默认情况下，实例元数据服务会返回 JSON 格式的数据 (`Content-Type: application/json`)。 但是，如果提出请求，不同 API 可以其他格式返回数据。
下表是有关 API 可支持的其他数据格式的参考。

API | 默认数据格式 | 其他格式
--------|---------------------|--------------
/instance | json | text
/scheduledevents | json | 无

若要访问非默认响应格式，在请求中将所请求格式指定为查询字符串参数。 例如：

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

### <a name="security"></a>“安全”
此实例元数据服务终结点只能从不可路由的 IP 地址上正在运行的虚拟机实例中访问。 此外，任何包含`X-Forwarded-For`标头的请求都会被服务拒绝。
请求必须包含 `Metadata: true` 标头，以确保实际请求是直接计划好的，而不是无意重定向的一部分。 

### <a name="error"></a>错误
如果找不到某个数据元素，或者请求的格式不正确，则实例元数据服务返回标准 HTTP 错误。 例如：

HTTP 状态代码 | 原因
----------------|-------
200 正常 |
400 错误的请求 | 缺少 `Metadata: true` 标头
404 未找到 | 请求的元素不存在 
不允许 405 方法 | 仅支持 `GET` 和 `POST` 请求
429 请求次数过多 | 该 API 当前支持每秒最多 5 个查询
500 服务错误     | 请稍后重试

### <a name="examples"></a>示例

> [!NOTE] 
> 所有 API 响应都是 JSON 字符串。 以下所有响应示例都以美观的形式输出以提高可读性。

#### <a name="retrieving-network-information"></a>检索网络信息

**请求**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

**响应**

> [!NOTE] 
> 此响应是 JSON 字符串。 以下示例响应显示清晰，可供阅读。

```json
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a>检索公共 IP 地址

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>检索实例的所有元数据

**请求**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-12-01"
```

**响应**

> [!NOTE] 
> 此响应是 JSON 字符串。 以下示例响应显示清晰，可供阅读。

```json
{
  "compute": {
    "location": "chinanorth",
    "name": "avset2",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "placementGroupId": "",
    "platformFaultDomain": "1",
    "platformUpdateDomain": "1",
    "publisher": "Canonical",
    "resourceGroupName": "myrg",
    "sku": "16.04-LTS",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "",
    "version": "16.04.201708030",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_D1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.2.5",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.2.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3A36DDED"
      }
    ]
  }
}
```

<!-- Notice: Zone is not Available on Moocake-->

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>在 Windows 虚拟机中检索元数据

**请求**

可以通过 Powershell 实用工具 `curl` 在 Windows 中检索实例元数据： 

```bash
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-08-01 | select -ExpandProperty Content
```

或通过 `Invoke-RestMethod`cmdlet：

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-08-01 -Method get 
```

**响应**

> [!NOTE] 
> 此响应是 JSON 字符串。 以下示例响应显示清晰，可供阅读。

```json
{
  "compute": {
    "location": "chinanorth",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "MicrosoftSQLServer",
    "sku": "Enterprise",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmSize": "Standard_DS2"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": [

          ]
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a>实例元数据数据类别
可通过实例元数据服务获取以下数据类别：

数据 | 说明 | 引入的版本 
-----|-------------|-----------------------
location | VM 在其中运行的 Azure 区域 | 2017-04-02 
name | VM 的名称 | 2017-04-02
offer | 为 VM 映像提供信息。 此值只适用于 Azure 映像库中部署的图像。 | 2017-04-02
发布者 | VM 映像的发布者 | 2017-04-02
sku | VM 映像的特定 SKU | 2017-04-02
版本 | VM 映像的版本 | 2017-04-02
osType | Linux 或 Windows | 2017-04-02
platformUpdateDomain |  正在运行 VM 的[更新域](manage-availability.md) | 2017-04-02
platformFaultDomain | 正在运行 VM 的[容错域](manage-availability.md) | 2017-04-02
vmId | VM 的[唯一标识符](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) | 2017-04-02
vmSize | [VM 大小](sizes.md) | 2017-04-02
subscriptionId | 虚拟机的 Azure 订阅 | 2017-08-01
标记 | 虚拟机的[标记](../../azure-resource-manager/resource-group-using-tags.md)  | 2017-08-01
resourceGroupName | 虚拟机的[资源组](../../azure-resource-manager/resource-group-overview.md) | 2017-08-01
计划 | 在 Azure 市场映像中 VM 的[计划](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan)，包含名称、产品和发布者 | 2018-04-02
publicKeys | 公钥的集合 [https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#sshpublickey]，已分配给 VM 和路径 | 2018-04-02
vmScaleSetName | 虚拟机规模集的[虚拟机规模集名称](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) | 2017-12-01
ipv4/privateIpAddress | VM 的本地 IPv4 地址 | 2017-04-02
ipv4/publicIpAddress | VM 的公共 IPv4 地址 | 2017-04-02
subnet/address | VM 的子网地址 | 2017-04-02 
subnet/prefix | 子网前缀，例如 24 | 2017-04-02 
macAddress | VM mac 地址 | 2017-04-02 
scheduledevents | 请参阅[计划事件](scheduled-events.md) | 2017-08-01

<!-- Not Available on Placement Group-->
<!-- Not Available on Availability Zone -->
<!-- Not available on ipv6 Mooncake -->
<!-- Not Available on MSI -->

## <a name="example-scenarios-for-usage"></a>用法的示例方案  

### <a name="tracking-vm-running-on-azure"></a>跟踪 Azure 上正在运行的 VM

作为服务提供商，可能需要跟踪运行软件的 VM 数目，或者代理需要跟踪 VM 的唯一性。 为了能够获取 VM 的唯一 ID，请使用实例元数据服务中的 `vmId` 字段。

**请求**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**响应**

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>基于容错/更新域放置容器、数据分区 

对于某些方案，不同数据副本的放置至关重要。 例如，对于 [HDFS 副本放置](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps)或者对于通过 [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) 放置容器，可能需要知道正在运行 VM 的 `platformFaultDomain` 和 `platformUpdateDomain`。

<!-- Not Available on  [Availability Zones](../../availability-zones/az-overview.md)-->

可以直接通过实例元数据服务查询此数据。

**请求**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text" 
```

**响应**

```
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a>在支持案例期间获取有关 VM 的详细信息 

作为服务提供商，你可能会接到支持电话，了解有关 VM 的详细信息。 请求客户共享计算元数据可以提供基本信息，以支持专业人员了解有关 Azure 上的 VM 类型。 

**请求**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-08-01"
```

**响应**

> [!NOTE] 
> 此响应是 JSON 字符串。 以下示例响应显示清晰，可供阅读。

```json
{
  "compute": {
    "location": "chinanorth",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="getting-azure-environment-where-the-vm-is-running"></a>获取 VM 所在的 Azure 环境 

Azure 具有各种主权云，如 Azure 中国云。 有时你需要使用 Azure 环境来做出一些运行时决策。 以下示例演示如何实现此目的。

**请求**

> [!NOTE] 
> 需要安装 jq。 

```bash
  metadata=$(curl "http://169.254.169.254/metadata/instance/compute?api-version=2018-02-01" -H "Metadata:true")
  endpoints=$(curl "https://management.chinacloudapi.cn/metadata/endpoints?api-version=2017-12-01")

  location=$(echo $metadata | jq .location -r)

  is_ww=$(echo $endpoints | jq '.cloudEndpoint.public.locations[]' -r | grep -w $location)
  is_us=$(echo $endpoints | jq '.cloudEndpoint.usGovCloud.locations[]' -r | grep -w $location)
  is_cn=$(echo $endpoints | jq '.cloudEndpoint.chinaCloud.locations[]' -r | grep -w $location)
  is_de=$(echo $endpoints | jq '.cloudEndpoint.germanCloud.locations[]' -r | grep -w $location)

  environment="Unknown"
  if [ ! -z $is_ww ]; then environment="AzureCloud"; fi
  if [ ! -z $is_us ]; then environment="AzureUSGovernment"; fi
  if [ ! -z $is_cn ]; then environment="AzureChinaCloud"; fi
  if [ ! -z $is_de ]; then environment="AzureGermanCloud"; fi

  echo $environment
```

<!-- Line 411 should be "AzureCloud"-->

### <a name="failover-clustering-in-windows-server"></a>Windows Server 中的故障转移群集

在某些情况下，在使用故障转移群集查询实例元数据服务时，必须向路由表添加路由。

1. 使用管理员特权打开命令提示符。

2. 运行以下命令，并记下 IPv4 路由表中网络目标 (`0.0.0.0`) 接口的地址。

```bat
route print
```

> [!NOTE] 
> 为简单起见，启用了故障转移群集的 Windows Server VM 的以下示例输出仅包含 IPv4 路由表。

```bat
IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0         10.0.1.1        10.0.1.10    266
         10.0.1.0  255.255.255.192         On-link         10.0.1.10    266
        10.0.1.10  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.15  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.63  255.255.255.255         On-link         10.0.1.10    266
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      169.254.0.0      255.255.0.0         On-link     169.254.1.156    271
    169.254.1.156  255.255.255.255         On-link     169.254.1.156    271
  169.254.255.255  255.255.255.255         On-link     169.254.1.156    271
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     169.254.1.156    271
        224.0.0.0        240.0.0.0         On-link         10.0.1.10    266
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     169.254.1.156    271
  255.255.255.255  255.255.255.255         On-link         10.0.1.10    266
```

3. 运行以下命令并使用网络目标 (`0.0.0.0`) 接口的地址，在此示例中为 `10.0.1.10`。

```bat
route add 169.254.169.254/32 10.0.1.10 metric 1 -p
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a>使用 VM 中的不同语言调用元数据服务的示例 

语言 | 示例 
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb
Go  | https://github.com/Microsoft/azureimds/blob/master/imdssample.go            
Python   | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
C++      | https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp
C#       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
Javascript | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh
Perl       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.pl
Java       | https://github.com/Microsoft/azureimds/blob/master/imdssample.java
Visual Basic | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.vb
Puppet | https://github.com/keirans/azuremetadata

## <a name="faq"></a>常见问题
1. 我收到错误 `400 Bad Request, Required metadata header not specified`。 这是什么意思呢？
   * 实例元数据服务需要在请求中传递标头 `Metadata: true`。 将该标头传入 REST 调用将允许访问实例元数据服务。 
2. 为什么我无法获取我的 VM 的计算信息？
   * 当前实例元数据服务仅支持 Azure Resource Manager 创建的实例。 将来可能会添加对云服务 VM 的支持。
3. 我刚才通过 Azure Resource Manager 创建了我的虚拟机。 为什么我无法看到计算元数据信息？
   * 对于 2016 年 9 月之后创建的所有 VM，请添加[标记](../../azure-resource-manager/resource-group-using-tags.md)以开始查看计算元数据。 对于早期 VM（在 2016 年 9 月之前创建），请在 VM 中添加/删除扩展或数据磁盘以刷新元数据。
4. 我看不到为新版本填充的任何数据
   * 对于 2016 年 9 月之后创建的所有 VM，请添加[标记](../../azure-resource-manager/resource-group-using-tags.md)以开始查看计算元数据。 对于早期 VM（在 2016 年 9 月之前创建），请在 VM 中添加/删除扩展或数据磁盘以刷新元数据。
5. 我为什么会收到错误 `500 Internal Server Error`？
   * 请根据指数后退系统重试请求。 如果问题持续出现，请联系 Azure 支持部门。
6. 在何处共享其他问题/评论？
   * 在 https://www.azure.cn/support/contact/ 上发送评论。
7. 这是否适用于虚拟机规模集实例？
   * 是的，元数据服务可用于规模集实例。 
8. 如何获取服务支持？
   * 若要获取服务支持，请针对无法在长时间重试后获得元数据响应的 VM，在 Azure 门户中创建支持问题。 
9. 调用服务时请求超时？
   * 必须从分配给 VM 的网卡的主 IP 地址进行元数据调用，此外，在已更改路由的情况下，网卡外必须存在地址 169.254.0.0/16 的路由。
10. 我更新了虚拟机规模集中的标记，但与 VM 不同，它们未显示在实例中，这是怎么回事？
   * 目前，对于规模集，仅在重启/重置映像/或对实例的磁盘更改时，向 VM 显示标记。 

   <!-- Not Available on New Support Request on Mooncake ![Instance Metadata Support](./media/instance-metadata-service/InstanceMetadata-support.png)-->

## <a name="next-steps"></a>后续步骤

- 详细了解[计划事件](scheduled-events.md)

<!--Update_Description: update meta properties, wording update -->