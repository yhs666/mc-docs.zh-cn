---
title: "Azure 实例元数据服务概述 | Azure"
description: "一个 RESTful 接口，用于获取有关 VM 计算、网络和即将发生的维护事件的信息。"
services: virtual-machines-windows, virtual-machines-linux,virtual-machines-scale-sets, cloud-services
documentationcenter: 
author: harijay
manager: timlt
editor: 
tags: 
ms.service: azure-instancemetadataservice
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 03/27/2017
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: 69e81e7c251f72a1efbd25e7594636e77948ec80
ms.sourcegitcommit: f119d4ef8ad3f5d7175261552ce4ca7e2231bc7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# Azure 实例元数据服务
<a id="azure-instance-metadata-service" class="xliff"></a> 

Azure 实例元数据服务提供有关可用于管理和配置虚拟机的正在运行的虚拟机实例的信息。
这包括 SKU、网络配置和即将发生的维护事件等信息。 若要详细了解可用信息类型，请参阅[元数据类别](#instance-metadata-data-categories)。

Azure 的实例元数据服务是一个 REST 终结点，所有创建的 IaaS VM 可通过 [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) 访问此终结点。 该终结点位于已知不可路由的 IP 地址 (`169.254.169.254`)，该地址只能从 VM 中访问。

## 服务可用性
<a id="service-availability" class="xliff"></a>
Azure China 公共预览版中提供该服务。

## 使用情况
<a id="usage" class="xliff"></a>

### 版本控制
<a id="versioning" class="xliff"></a>
已对实例元数据服务进行了版本控制。 版本是必需的，当前版本为 `2017-04-02`。

> [!NOTE] 
> 支持的计划事件的前一预览版 {latest} 发布为 api-version。 此格式不再受支持，并且将在未来弃用。

添加更新的版本时，早期版本仍可供访问以保持兼容性（如果脚本依赖于特定的数据格式）。 不过请注意，当前的预览版本 (2017-03-01) 在服务正式发布之后可能会不可用。

### 使用标头
<a id="using-headers" class="xliff"></a>
查询实例元数据服务时，必须提供标头 `Metadata: true` 以确保不会意外重定向请求。

### 检索元数据
<a id="retrieving-metadata" class="xliff"></a>

实例元数据可用于运行使用 [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) 创建/管理的 VM。 使用以下请求访问虚拟机实例的所有数据类别：

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> 所有实例元数据查询都区分大小写。

### 数据输出
<a id="data-output" class="xliff"></a>
默认情况下，实例元数据服务会返回 JSON 格式的数据 (`Content-Type: application/json`)。 但是，可请求不同的 API 返回不同格式的数据。
下表是有关 API 可支持的其他数据格式的参考。

API | 默认数据格式 | 其他格式
--------|---------------------|--------------
/instance | json | text
/scheduledevents | json | 无

若要访问非默认响应格式，在请求中将所请求格式指定为查询字符串参数。 例如：

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### “安全”
<a id="security" class="xliff"></a>
此实例元数据服务终结点只能从不可路由的 IP 地址上正在运行的虚拟机实例中访问。 此外，任何包含`X-Forwarded-For`标头的请求都会被服务拒绝。
还要求请求包含 `Metadata: true` 标头以确保实际请求是直接计划好的，不属于意外重定向。 

### 错误
<a id="error" class="xliff"></a>
如果找不到某个数据元素，或者请求的格式不正确，则实例元数据服务返回标准 HTTP 错误。 例如：

HTTP 状态代码 | 原因
----------------|-------
200 正常 |
400 错误的请求 | 缺少 `Metadata: true` 标头
404 未找到 | 请求的元素不存在 
不允许 405 方法 | 仅支持 `GET` 和 `POST` 请求
429 请求次数过多 | 该 API 当前支持每秒最多 5 个查询
500 服务错误     | 请稍后重试

### 示例
<a id="examples" class="xliff"></a>

> [!NOTE] 
> 所有 API 响应都是 JSON 字符串。 以下所有响应示例都以美观的形式输出以提高可读性。

#### 检索网络信息
<a id="retrieving-network-information" class="xliff"></a>

**请求**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-04-02"
```

**响应**

> [!NOTE] 
> 此响应是 JSON 字符串。 以下响应示例以美观的形式输出以提高可读性。

```
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

#### 检索公共 IP 地址
<a id="retrieving-public-ip-address" class="xliff"></a>

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### 检索实例的所有元数据
<a id="retrieving-all-metadata-for-an-instance" class="xliff"></a>

**请求**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

**响应**

> [!NOTE] 
> 此响应是 JSON 字符串。 以下响应示例以美观的形式输出以提高可读性。

```
{
  "compute": {
    "location": "chinaeast",
    "name": "IMDSSample",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "Canonical",
    "sku": "16.04.0-LTS",
    "version": "16.04.201610200",
    "vmId": "5d33a910-a7a0-4443-9f01-6a807801b29b",
    "vmSize": "Standard_A1"
  },
  "network": {
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
}
```

#### 在 Windows 虚拟机中检索元数据
<a id="retrieving-metadata-in-windows-virtual-machine" class="xliff"></a>

**请求**

可以通过 Powershell 实用工具 `curl` 在 Windows 中检索实例元数据： 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

或通过 `Invoke-RestMethod`cmdlet：

```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

**响应**

> [!NOTE] 
> 此响应是 JSON 字符串。 以下响应示例以美观的形式输出以提高可读性。

```
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

## 实例元数据数据类别
<a id="instance-metadata-data-categories" class="xliff"></a>
可通过实例元数据服务获取以下数据类别：

数据 | 说明
-----|------------
location | VM 在其中运行的 Azure 区域
名称 | VM 的名称 
offer | 为 VM 映像提供信息。 此值只适用于 Azure 映像库中部署的图像。
发布者 | VM 映像的发布者
sku | VM 映像的特定 SKU  
版本 | VM 映像的版本 
osType | Linux 或 Windows 
platformUpdateDomain |  正在运行 VM 的[更新域](virtual-machines-windows-manage-availability.md)
platformFaultDomain | 正在运行 VM 的[容错域](virtual-machines-windows-manage-availability.md)
vmId | VM 的[唯一标识符](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)
vmSize | [VM 大小](virtual-machines-windows-sizes.md)
ipv4/privateIpAddress | VM 的本地 IPv4 地址 
ipv4/publicIpAddress | VM 的公共 IPv4 地址
subnet/address | VM 的子网地址
subnet/prefix | 子网前缀，例如 24
ipv6/ipAddress | VM 的本地 IPv6 地址
macAddress | VM mac 地址 
scheduledevents | 当前在公共预览版中提供。请参阅 [scheduledevents](virtual-machines-scheduled-events.md)

## 使用情况的示例方案
<a id="example-scenarios-for-usage" class="xliff"></a>  

### 跟踪 Azure 上正在运行的 VM
<a id="tracking-vm-running-on-azure" class="xliff"></a>

作为服务提供商，可能需要跟踪运行软件的 VM 数目，或者代理需要跟踪 VM 的唯一性。 为了能够获取 VM 的唯一 ID，请使用实例元数据服务中的 `vmId` 字段。

**请求**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

**响应**

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### 基于容错/更新域放置容器、数据分区
<a id="placement-of-containers-data-partitions-based-faultupdate-domain" class="xliff"></a> 

对于某些方案，不同数据副本的放置至关重要。 例如，对于 [HDFS 副本放置](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps)或者对于通过 [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) 放置容器，可能需要知道正在运行 VM 的 `platformFaultDomain` 和 `platformUpdateDomain`。
可以直接通过实例元数据服务查询此数据。

**请求**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

**响应**

```
0
```

### 在支持案例期间获取有关 VM 的详细信息
<a id="getting-more-information-about-the-vm-during-support-case" class="xliff"></a> 

作为服务提供商，你可能会接到支持电话，了解有关 VM 的详细信息。 请求客户共享计算元数据可以提供基本信息，以支持专业人员了解有关 Azure 上的 VM 类型。 

**请求**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

**响应**

> [!NOTE] 
> 此响应是 JSON 字符串。 以下响应示例以美观的形式输出以提高可读性。

```
{
  "compute": {
    "location": "CentralUS",
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

### 使用 VM 中的不同语言调用元数据服务的示例
<a id="examples-of-calling-metadata-service-using-different-languages-inside-the-vm" class="xliff"></a> 

语言 | 示例 
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb
Go Lan   | https://github.com/Microsoft/azureimds/blob/master/imdssample.go            
Python   | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
C++      | https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp
C#       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
Javascript | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
Powershell | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh

## 常见问题
<a id="faq" class="xliff"></a>
1. 我收到错误 `400 Bad Request, Required metadata header not specified`。 这是什么意思呢？
   * 实例元数据服务需要在请求中传递标头 `Metadata: true`。 将该标头传入 REST 调用将允许访问实例元数据服务。 
2. 为什么我无法获取我的 VM 的计算信息？
   * 当前实例元数据服务仅支持 Azure Resource Manager 创建的实例。 将来可能添加对云服务 VM 的支持。
3. 我刚才通过 Azure Resource Manager 创建了我的虚拟机。 为什么我无法看到计算元数据信息？
   * 对于 2016 年 9 月之后创建的所有 VM，请添加[标记](../azure-resource-manager/resource-group-using-tags.md)以开始查看计算元数据。 对于早期 VM（在 2016 年 9 月之前创建），请在 VM 中添加/删除扩展或数据磁盘以刷新元数据。
4. 我为什么会收到错误 `500 Internal Server Error`？
   * 请基于指数撤回系统重试请求。 如果问题持续出现，请联系 Azure 支持部门。
5. 在何处共享其他问题/评论？
   * 请将评论发布到 http://feedback.azure.com。
7. 这是否适用于虚拟机规模集实例？
   * 是的，元数据服务可用于规模集实例。 
6. 如何获取服务支持？
   * 若要获取服务支持，请针对无法在长时间重试后获得元数据响应的 VM，在 Azure 门户中创建支持问题。 

   ![实例元数据支持](./media/virtual-machines-instancemetadataservice-overview/InstanceMetadata-support.png)

## 后续步骤
<a id="next-steps" class="xliff"></a>

- 在由实例元数据服务提供的公共预览版中，详细了解 [scheduledevents](virtual-machines-scheduled-events.md) API。