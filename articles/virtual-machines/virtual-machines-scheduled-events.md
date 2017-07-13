---
title: "通过 Azure 元数据服务使用计划事件 | Azure"
description: "应对虚拟机上的有影响事件（在其发生之前）。"
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 12/10/2016
ms.date: 07/10/2017
ms.author: v-dazen
ms.openlocfilehash: 9f0dbd6f14b9b7513f684da3448d270f5df5cdcb
ms.sourcegitcommit: b3e981fc35408835936113e2e22a0102a2028ca0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/30/2017
---
# Azure 元数据服务 - 计划事件（预览）
<a id="azure-metadata-service---scheduled-events-preview" class="xliff"></a>

> [!NOTE] 
> 同意使用条款即可使用预览版。
>

计划事件是 Azure 元数据服务中的其中一个子服务。 它负责显示有关即将发生的事件（例如，重新启动）的信息，使应用程序可以为其做准备并限制中断。 它可用于所有 Azure 虚拟机类型（包括 PaaS 和 IaaS）。 计划事件为虚拟机提供了执行预防性任务的时间，将事件的影响降到最低。 

## 简介 - 为什么要有计划事件？
<a id="introduction---why-scheduled-events" class="xliff"></a>

通过计划事件，可以采取措施限制由平台启动的维护或由用户启动的操作对服务带来的影响。 

使用复制技术保持状态的多实例工作负荷可能易受到跨多个实例频繁发生的服务中断的影响。 此类中断可能导致任务开销昂贵（例如，重新生成索引）甚至副本丢失。 

在很多其他情况下，通过正常关闭序列（如完成或取消正在进行的事务）、将任务重新分配给群集中的其他 VM（手动故障转移）或从网络负载均衡器池中删除虚拟机，可能都可提高服务的整体可用性。 

有时通知管理员即将发生的事件或记录此类事件也可帮助提高在云中托管的应用程序的可服务性。

Azure 元数据服务在以下用例中显示计划事件：
-   平台启动的维护（例如，主机 OS 部署）
-   用户启动的调用（例如，用户重启或重新部署 VM）

## 计划事件 - 基础知识
<a id="scheduled-events---the-basics" class="xliff"></a>  

Azure 元数据服务公开在 VM 中使用可访问的 REST 终结点运行虚拟机的相关信息。 该信息通过不可路由的 IP 提供，因此不会在 VM 外部公开。

### 范围
<a id="scope" class="xliff"></a>
计划事件将显示到云服务中的所有虚拟机或可用性集中的所有虚拟机上。 因此，应查看事件中的 `Resources` 字段以确定将受到影响的 VM。 

### 发现终结点
<a id="discovering-the-endpoint" class="xliff"></a>
如果在虚拟网络 (VNet) 中创建虚拟机，可从不可路由 IP `169.254.169.254` 获得元数据服务。
如果不在虚拟网络中创建虚拟机（云服务和经典 VM 的默认情况），需要使用其他逻辑发现可使用的终结点。 请参阅此示例，了解如何[发现主机终结点](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm)。

### 版本控制
<a id="versioning" class="xliff"></a> 
已对实例元数据服务进行了版本控制。 版本是必需的，当前版本为 `2017-03-01`。

> [!NOTE] 
> 支持的计划事件的前一预览版 {latest} 发布为 api-version。 此格式不再受支持，并且将在未来弃用。

### 使用标头
<a id="using-headers" class="xliff"></a>
查询元数据服务时，必须提供标头 `Metadata: true` 以确保不会在无意中重定向该请求。

### 启用计划事件
<a id="enabling-scheduled-events" class="xliff"></a>
首次请求计划事件时，Azure 会在虚拟机上隐式启用该功能。 因此，第一次调用时应该会延迟响应最多两分钟。

### 通过用户启动的操作测试逻辑
<a id="testing-your-logic-with-user-initiated-operations" class="xliff"></a>
若要测试逻辑，可以使用 Azure 门户、API、CLI 或 PowerShell 启动生成计划事件的操作。 重启虚拟机会生成事件类型为“`Reboot`”的计划事件。 重新部署虚拟机会生成事件类型为“`Redeploy`”的计划事件。
在这两种情况下，用户启动的操作都需要更长的时间才能完成，因为计划事件要为应用程序留出更多的时间来正常关闭。 

## 使用 API
<a id="using-the-api" class="xliff"></a>

### 查询事件
<a id="query-for-events" class="xliff"></a>
只需进行以下调用即可查询计划事件：

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

响应包含计划事件的数组。 数组为空意味着目前没有计划事件。
如果有计划事件，响应会包含事件的数组： 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### 事件属性
<a id="event-properties" class="xliff"></a>
|属性  |  说明 |
| - | - |
| EventId | 此事件的全局唯一标识符。 <br><br> 示例： <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| EventType | 此事件造成的影响。 <br><br> 值： <br><ul><li> `Freeze`：虚拟机计划暂停数秒。 CPU 将暂停，但对内存、打开的文件或网络连接没有影响。 <li>`Reboot`：虚拟机将计划重新启动（非持久性内存将丢失）。 <li>`Redeploy`：虚拟机计划移到另一个节点（临时磁盘将丢失）。 |
| ResourceType | 此事件影响的资源的类型。 <br><br> 值： <ul><li>`VirtualMachine`|
| 资源| 此事件影响的资源的列表。 它保证最多只能包含一个[更新域](windows/manage-availability.md)的计算机，但可能不包含该更新域中的所有计算机。 <br><br> 示例： <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| 事件状态 | 此事件的状态。 <br><br> 值： <ul><li>`Scheduled`：此事件计划在 `NotBefore` 属性指定的时间之后启动。<li>`Started`：此事件已启动。</ul> 不提供 `Completed` 或类似状态；事件完成后，将不再返回事件。
| NotBefore| 此事件可能会在之后启动的时间。 <br><br> 示例： <br><ul><li> 2016-09-19T18:29:47Z  |

### 事件计划
<a id="event-scheduling" class="xliff"></a>
将根据事件类型为每个事件计划将来的最小量时间。 此时间反映在某个事件的 `NotBefore` 属性上。 

|EventType  | 最小值通知 |
| - | - |
| 冻结| 15 分钟 |
| 重新启动 | 15 分钟 |
| 重新部署 | 10 分钟 |

### 启动事件（加快）
<a id="starting-an-event-expedite" class="xliff"></a>

了解即将发生的事件并完成正常关闭逻辑后，可以通过使用 `EventId` 对元数据服务进行 `POST` 调用来批准未完成的事件。 这指示 Azure 可以缩短最小通知时间（如可能）。 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> 如果确认某个事件，则将允许该事件中的所有 `Resources` 继续进行，而不仅仅是确认该事件的虚拟机。 因此，可以选择一个指挥计算机来协调该确认，为简单起见，可选择 `Resources` 字段中的第一个计算机。

## 示例
<a id="samples" class="xliff"></a>

### PowerShell 示例
<a id="powershell-sample" class="xliff"></a> 

下例将查询计划事件的元数据服务器并审核所有未完成的事件。

```PowerShell
# How to get scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 

    # Convert to JSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with the following: `n" $approvalString

    # Post approval string to scheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function HandleScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 

# Get events
$scheduledEvents = GetScheduledEvents $scheduledEventURI

# Handle events however is best for your service
HandleScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        ApproveScheduledEvent $event.EventId $scheduledEvents.DocumentIncarnation $scheduledEventURI 
    }
}
``` 

### C\# 示例
<a id="c-sample" class="xliff"></a> 

下例是关于与元数据服务进行通信的简单客户端。

```csharp
public class ScheduledEventsClient
{
    private readonly string scheduledEventsEndpoint;
    private readonly string defaultIpAddress = "169.254.169.254"; 

    // Set up the scheduled events URI for a VNET-enabled VM
    public ScheduledEventsClient()
    {
        scheduledEventsEndpoint = string.Format("http://{0}/metadata/scheduledevents?api-version=2017-03-01", defaultIpAddress);
    }

    // Get events
    public string GetScheduledEvents()
    {
        Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Metadata", "true");
            return webClient.DownloadString(cloudControlUri);
        }   
    }

    // Approve events
    public void ApproveScheduledEvents(string jsonPost)
    {
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Content-Type", "application/json");
            webClient.UploadString(scheduledEventsEndpoint, jsonPost);
        }
    }
}
```

无法使用以下数据结构表示计划事件：

```csharp
public class ScheduledEventsDocument
{
    public string DocumentIncarnation;
    public List<CloudControlEvent> Events { get; set; }
}

public class CloudControlEvent
{
    public string EventId { get; set; }
    public string EventStatus { get; set; }
    public string EventType { get; set; }
    public string ResourceType { get; set; }
    public List<string> Resources { get; set; }
    public DateTime? NotBefore { get; set; }
}

public class ScheduledEventsApproval
{
    public string DocumentIncarnation;
    public List<StartRequest> StartRequests = new List<StartRequest>();
}

public class StartRequest
{
    [JsonProperty("EventId")]
    private string eventId;

    public StartRequest(string eventId)
    {
        this.eventId = eventId;
    }
}
```

下例将查询计划事件的元数据服务器并审核所有未完成的事件。

```csharp
public class Program
{
    static ScheduledEventsClient client;

    static void Main(string[] args)
    {
        client = new ScheduledEventsClient();

        while (true)
        {
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter to approve executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval()
            {
                DocumentIncarnation = scheduledEventsDocument.DocumentIncarnation
            };

            foreach (CloudControlEvent event in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(event.EventId));
            }

            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.ApproveScheduledEvents(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter to repeat\n\n");
            Console.ReadLine();
            Console.Clear();
        }
    }

    private static void HandleEvents(List<CloudControlEvent> events)
    {
        // Add logic for handling events here
    }
}
```

### Python 示例
<a id="python-sample" class="xliff"></a> 

下例将查询计划事件的元数据服务器并审核所有未完成的事件。

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            # Add logic for handling events here

def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)

if __name__ == '__main__':
  main()
  sys.exit(0)
```

## 后续步骤
<a id="next-steps" class="xliff"></a> 

- 有关 API 的更多信息，请参阅[实例元数据服务](virtual-machines-instancemetadataservice-overview.md)。
- 了解 [Azure 中 Windows 虚拟机的计划内维护](windows/planned-maintenance.md)。
- [Azure 中 Linux 虚拟机的计划内维护](linux/planned-maintenance.md)。
