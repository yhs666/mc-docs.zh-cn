---
title: 检查池和节点错误 - Azure Batch
description: 创建池和节点时要检查的错误以及如何避免错误
services: batch
author: lingliw
ms.author: v-lingwu
ms.date: 1/3/2019
ms.topic: conceptual
ms.openlocfilehash: 9f04128dbf3311f0fc9171884efb39b47311002f
ms.sourcegitcommit: f46e1f7a5d582bb9663bfaee8087b233eb822e17
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53996504"
---
# <a name="check-for-pool-and-node-errors"></a>检查池和节点错误

创建和管理 Azure Batch 池时，某些操作是即时发生的。 但是，某些操作是异步的，在后台运行。 这些操作可能需要几分钟才能完成。

检测即时发生的失败操作非常简单，因为 API、CLI 或 UI 会立即返回任何错误。

本文介绍可对池和池节点执行的后台操作， 其中说明了如何检测和避免错误。

## <a name="pool-errors"></a>池错误

### <a name="resize-timeout-or-failure"></a>调整大小超时或失败

创建新池或调整现有池大小时，需指定目标节点数。  该操作是即时完成的，但实际分配新节点或删除现有节点可能需要几分钟时间。  在 [create](https://docs.microsoft.com/rest/api/batchservice/pool/add) 或 [resize](https://docs.microsoft.com/rest/api/batchservice/pool/resize) API 中指定大小调整超时。 如果 Batch 在大小调整超时期限内无法获取目标节点数，它会停止该操作。 池将进入稳定状态并报告大小调整错误。

最近执行的评估的 [ResizeError](https://docs.microsoft.com/rest/api/batchservice/pool/get#resizeerror) 属性将报告大小调整超时，并列出发生的任何错误。

调整大小超时的常见原因包括：
- 调整大小超时过短
  - 在大多数情况下，默认超时 15 分钟已足够长，在此期限内可以完成池节点的分配或删除。
  - 若要分配大量的节点，我们建议将大小调整超时设置为 30 分钟。 例如，要在 Azure 市场映像中将大小调整为 1,000 个以上的节点，或者在自定义 VM 映像中将大小调整为 300 个以上的节点时。
- 核心配额不足
  - Batch 帐户在所有池中可分配的核心数有限制。 一旦达到该配额，Batch 就会停止分配节点。 [可以提高](https://docs.microsoft.com/azure/batch/batch-quota-limit)核心配额，使 Batch 能够分配更多的节点。
- 当[池处于虚拟网络](https://docs.microsoft.com/azure/batch/batch-virtual-network)，子网 IP 不足
  - 虚拟网络子网必须能够提供可分配到每个请求的池节点的、尚未分配的足够 IP 地址。 否则无法创建节点。
- 当[池处于虚拟网络](https://docs.microsoft.com/azure/batch/batch-virtual-network)，资源不足
  - 你可能会在 Batch 帐户所在的同一订阅中创建负载均衡器、公共 IP 和网络安全组等资源。 请检查这些资源的订阅配额是否足够。
- 使用自定义 VM 映像的大型池
  - 使用自定义 VM 映像的大型池可能需要更长的时间进行分配，并且可能发生大小调整超时。  有关限制和配置的建议，请参阅[使用自定义映像创建虚拟机池](https://docs.microsoft.com/azure/batch/batch-custom-images)。

### <a name="automatic-scaling-failures"></a>自动缩放功能

也可以将 Azure Batch 设置为自动缩放池中的节点数。 定义[池自动缩放公式](https://docs.microsoft.com/azure/batch/batch-automatic-scaling)的参数。 Batch 服务使用该公式定期评估池中的节点数，并设置新的目标数。 可能会发生以下类型问题：

- 自动缩放评估失败。
- 生成的大小调整操作失败和超时。
- 自动缩放公式的问题导致节点目标值不正确。 大小调整操作要么正常运行，要么超时。

可以使用 [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/pool/get#autoscalerun) 属性获取有关上次自动缩放评估的信息。 此属性将报告评估时间、值和结果，以及任何性能错误。

[池大小调整完成事件](https://docs.microsoft.com/azure/batch/batch-pool-resize-complete-event)捕获有关所有评估的信息。

### <a name="delete"></a>Delete

当你删除包含节点的池时，Batch 首先会删除节点， 然后再删除池对象本身。 可能需要等待数分钟才会删除池节点。

在删除过程中，Batch 会将[池状态](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate)设置为 **deleting**。 调用方应用程序可以使用 **state** 和 **stateTransitionTime** 属性来检测池删除时间是否过长。

## <a name="pool-compute-node-errors"></a>池计算节点错误

即使 Batch 在池中成功分配了节点，各种问题仍可能会导致某些节点不正常且不可用。 这些节点会产生费用。 请务必检测问题，以免为不可用的节点付费。

### <a name="start-task-failure"></a>启动任务失败

你可能想要为某个池指定可选的[启动任务](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask)。 与指定任何任务一样，可以使用命令行和资源文件从存储下载。 启动任务在启动后，将针对每个节点运行。 **waitForSuccess** 属性指定 Batch 是否要等到启动任务成功完成，然后才为节点计划任何任务。

如果已将节点配置为等待启动任务成功完成，但启动任务失败，将会发生什么情况？ 在这种情况下，该节点将不可用，但仍会产生费用。

可以使用顶级 [startTaskInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#starttaskinformation) 节点属性的 [result](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskexecutionresult) 和 [failureInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskfailureinformation) 属性来检测启动任务失败。

如果将 **waitForSuccess** 设置为 **true**，启动任务失败还会导致 Batch 将节点[状态](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate)设置为 **starttaskfailed**。

与任何任务一样，开始任务失败可能有许多原因。  若要进行故障排除，请检查 stdout、stderr 和其他任何特定于任务的日志文件。

### <a name="application-package-download-failure"></a>应用程序包下载失败

可为某个池指定一个或多个应用程序包。 在启动节点之后、计划任务之前，Batch 会将指定的包文件下载到每个节点并解压缩这些文件。 通常会将启动任务命令行与应用程序包结合使用。 例如，将文件复制到其他位置，或运行安装程序。

节点 [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) 属性将报告下载和解压缩应用程序包失败。 Batch 将节点状态设置为 **unusable**。

### <a name="node-in-unusable-state"></a>处于“不可用”状态的节点

Azure Batch 可能出于多种原因将[节点状态](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate)设置为 **unusable**。 将节点状态设置为 **unusable** 后，无法为该节点计划任务，但它仍会产生费用。

Batch 始终会尝试恢复不可用的节点，但根据具体的原因，恢复不一定能够成功。

如果 Batch 可以确定原因，则节点 [errors](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) 属性会报告此原因。

节点**不可用**的其他原因示例包括：

- 自定义 VM 映像无效。 例如，未正确准备某个映像。
- 由于基础结构故障或低级别升级而移动了 VM。 Batch 将恢复节点。

### <a name="node-agent-log-files"></a>节点代理日志文件

每个池节点上运行的 Batch 代理进程可以提供日志文件，如果你需要就池节点问题联系支持人员，这些日志可能很有帮助。 可以通过 Azure 门户、Batch Explorer 或 [API](https://docs.microsoft.com/rest/api/batchservice/computenode/uploadbatchservicelogs) 上传节点的日志文件。 最好是上传并保存日志文件。 以后，可以删除节点或池，以节省运行中节点的成本。

## <a name="next-steps"></a>后续步骤

检查是否已将应用程序设置为实施全面的错误检查，尤其是对于异步操作。 及时检测和诊断问题有时是很关键的。
