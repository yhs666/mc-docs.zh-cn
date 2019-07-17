---
title: 排查 SSIS Integration Runtime 中的包执行问题 | Microsoft Docs
description: 本文提供有关排查 SSIS Integration Runtime 中的 SSIS 包执行问题的指导
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 04/15/2019
ms.date: 07/08/2019
author: WenJason
ms.author: v-jay
ms.reviewer: sawinark
manager: digimobile
ms.openlocfilehash: 181fae8080df930eca51c4a2de2c80195f5e0b67
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571445"
---
# <a name="troubleshooting-package-execution-in-ssis-integration-runtime"></a>排查 SSIS Integration Runtime 中的包执行问题

本文描述了在执行 SSIS Integration Runtime 中的 SSIS 包时可能遇到的最常见错误、潜在的原因，以及解决这些错误的操作。

## <a name="where-can-i-find-logs-for-troubleshoot"></a>在何处可以找到用于故障排除的日志

* 可以使用 ADF 门户检查 SSIS 包执行活动的输出，包括执行结果、错误消息和操作 ID。 在[监视管道](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline)中可以找到详细信息

* SSIS 目录 (SSISDB) 可用于检查执行活动的详细日志。 在[监视正在运行的包和其他操作](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017)中可以找到详细信息

## <a name="common-errors-causes-and-solution"></a>常见错误、原因和解决方法

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>错误消息：`"Connection Timeout Expired."` 或 `"The service has encountered an error processing your request. Please try again."`

* 可能的原因和建议的操作：
  * 数据源/目标过载。 检查数据源/目标的负载，并查看它是否有足够的容量。 例如，如果使用的是 Azure SQL，而该数据库可能会超时，则我们建议考虑纵向扩展。
  * SSIS Integration Runtime 与数据源/目标之间的网络不稳定，当连接跨区域或者是在本地与 Azure 之间建立的时尤其如此。 建议执行以下步骤，在 SSIS 包中应用重试模式：
    * 确保 SSIS 包在失败时可以重新运行，且不产生负面影响（例如 数据丢失、数据重复等等）
    * 在“常规”选项卡中配置“执行 SSIS 包活动”的“重试”和“重试间隔”![在“常规”选项卡中设置属性](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)  
    * 对于 ADO.NET 和 OLEDB 源/目标组件，可以在 SSIS 包或 SSIS 活动的连接管理器中设置 ConnectRetryCount 和 ConnectRetryInterval

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-the-following-error-message-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>错误消息：`"ADO NET Source has failed to acquire the connection '...' with the following error message: "A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible."`

* 可能的原因和建议的操作：
  * 此问题通常意味着无法从 SSIS Integration Runtime 访问数据源/目标，这是由不同的原因造成的：
    * 确保正确传递数据源/目标的名称/IP
    * 确保已正确设置防火墙
    * 如果数据源/目标位于本地，请确保正确配置 vNet。
      * 可以通过在同一 vNet 中预配 Azure VM，来验证问题是否与 vNet 配置有关。 然后检查是否可以从 Azure VM 访问数据源/目标
      * 可以在[将 Azure-SSIS Integration Runtime 加入虚拟网络](join-azure-ssis-integration-runtime-virtual-network.md)中找到有关将 vNet 与 SSIS Integration Runtime 配合使用的更多详细信息

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-the-following-error-message-could-not-create-a-managed-connection-manager"></a>错误消息：“`ADO NET Source has failed to acquire the connection '...' with the following error message: "Could not create a managed connection manager.`”

* 可能的原因和建议的操作：
  * 在包中使用的 ADO.NET 提供程序未安装在 SSIS Integration Runtime 中。 可以使用“自定义安装”来安装该提供程序。 在[自定义 Azure SSIS Integration Runtime 的安装](how-to-configure-azure-ssis-ir-custom-setup.md)中可以找到有关“自定义安装”的更多详细信息

### <a name="error-message-the-connection--is-not-found"></a>错误消息：“`The connection '...' is not found`”

* 可能的原因和建议的操作：
  * 此错误可能是旧版 SSMS 中的某个已知问题造成的。 如果包中包含自定义组件（例如，SSIS Azure 功能包或第三方组件），而该组件未安装在使用 SSMS 执行部署的计算机上，则 SSMS 会删除该组件，导致出错。 将 [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) 升级到已解决该问题的最新版本。

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>错误消息：“磁盘上没有足够的空间”

* 可能的原因和建议的操作：
  * 此错误表示 SSIS Integration Runtime 节点中的本地磁盘空间已用尽。 检查你的包或自定义安装是否占用了过多的磁盘空间。
    * 如果包占用了磁盘，包执行完成后会释放磁盘空间。
    * 如果自定义安装占用了磁盘，则你需要停止 SSIS Integration Runtime，修改脚本，然后再次启动 SSIS Integration Runtime。 为自定义安装指定的整个 Azure Blob 容器将复制到 SSIS IR 节点，因此，请验证该容器中是否包含任何不必要的内容。

### <a name="error-message-cannot-open-file-"></a>错误消息：“无法打开文件 '...'”

* 可能的原因和建议的操作：
  * 当包执行在 SSIS Integration Runtime 中的本地磁盘内找不到文件时，将发生此错误。
    * 不建议在 SSIS Integration Runtime 中执行的包内使用绝对路径。 请改用当前执行工作目录 (.) 或临时文件夹 (%TEMP%)。
    * 如果需要在 SSIS Integration Runtime 节点上保留某些文件，建议通过[自定义安装](how-to-configure-azure-ssis-ir-custom-setup.md)准备文件。 执行完成后，系统会清理执行工作目录中的所有文件。
    * 另一种做法是改用 Azure 文件来存储 SSIS Integration Runtime 节点的文件。 在 [https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares) 中可以找到更多详细信息。

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>错误消息：“数据库 'SSISDB' 已达到大小配额”

* 可能的原因和建议的操作：
  * 创建 SSIS Integration Runtime 时在 Azure SQL 或托管实例中创建的 SSISDB 已达到其配额。
    * 请考虑增加数据库的 DTU 来解决此问题。 在 [https://docs.azure.cn/sql-database/sql-database-resource-limits-logical-server](https://docs.azure.cn/sql-database/sql-database-resource-limits-logical-server) 中可以找到详细信息
    * 检查包是否生成许多日志。 如果是，可以配置弹性作业来清理这些日志。 有关详细信息，请参阅[使用 Azure 弹性数据库作业清理 SSISDB 日志](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md)。

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>错误消息：“数据库的请求限制是 ...，现已达到该限制。”

* 可能的原因和建议的操作：
  * 如果在 SSIS Integration Runtime 中同时执行许多的包，可能会发生此错误，因为已达到 SSISDB 的请求限制。 请考虑增加 SSISDB 的 DTC 来解决此问题。 在 [https://docs.azure.cn/sql-database/sql-database-resource-limits-logical-server](https://docs.azure.cn/sql-database/sql-database-resource-limits-logical-server) 中可以找到详细信息

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>错误消息：“SSIS 操作失败并出现意外的操作状态: ...”

* 可能的原因和建议的操作：
  * 该错误主要是由某个暂时性错误导致的，因此请尝试重新运行包执行。 建议执行以下步骤，在 SSIS 包中应用重试模式：
    * 确保 SSIS 包在失败时可以重新运行，且不产生负面影响（例如数据丢失、数据重复等）
    * 在“常规”选项卡中配置“执行 SSIS 包活动”的“重试”和“重试间隔”![在“常规”选项卡中设置属性](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)  
    * 对于 ADO.NET 和 OLEDB 源/目标组件，可以在 SSIS 包或 SSIS 活动的连接管理器中设置 ConnectRetryCount 和 ConnectRetryInterval

### <a name="error-message-there-is-no-active-worker"></a>错误消息：“没有任何活动的辅助角色。”

* 可能的原因和建议的操作：
  * 此错误通常表示 SSIS Integration Runtime 处于不正常状态。 在 Azure 门户中检查状态和详细错误：[https://docs.azure.cn/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime](https://docs.azure.cn/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime)

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>错误消息：“集成运行时无法升级，最终将会停止工作，因为我们无法访问你为自定义安装提供的 Azure Blob 容器。”

* 当 SSIS Integration Runtime 无法访问针对自定义安装配置的存储时，将发生此错误。 检查提供的 SAS URI 有效且未过期。

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>错误消息：“Microsoft OLE DB Provider for Analysis Services。 ‘Hresult:0x80004005 说明:’COM 错误:COM 错误: mscorlib；某个调用的目标引发了异常”

* 可能的原因和建议的操作：
  * 一种可能的原因是为 Azure Analysis Services 身份验证配置了已启用 MFA 的用户名/密码，但 SSIS Integration Runtime 尚不支持此配置。 尝试使用服务主体进行 Azure Analysis Service 身份验证：
    1. 准备 AAS 的服务主体 [https://docs.azure.cn/analysis-services/analysis-services-service-principal](https://docs.azure.cn/analysis-services/analysis-services-service-principal)
    2. 在连接管理器中，配置“使用特定的用户名和密码”：将“AppID”设为用户名，将“clientSecret”设为密码

### <a name="package-takes-unexpected-long-time-to-execute"></a>包的执行意外地花费了很长时间

* 可能的原因和建议的操作：
  * 在 SSIS Integration Runtime 中计划了过多的包执行。 在这种情况下，所有这些执行将在队列中等待执行。
    * 每个 IR 的最大并行执行计数 = 节点计数 * 每个节点的最大并行执行数
    * 有关如何设置“节点计数”以及“每个节点的最大并行执行数”，请参阅[在 Azure 数据工厂中创建 Azure-SSIS Integration Runtime](create-azure-ssis-integration-runtime.md)。
  * SSIS Integration Runtime 已停止或处于不正常状态。 有关如何检查 SSIS Integration Runtime 状态和错误，请查看 [Azure-SSIS Integration Runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime)。
  * 如果你确认包执行在特定的时间内应可完成，则我们建议设置超时：![在“常规”选项卡上设置属性](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

### <a name="poor-performance-in-package-execution"></a>包的执行性能不佳

* 可能的原因和建议的操作：

  * 检查 SSIS Integration Runtime 是否与数据源和目标位于同一区域。

  * 启用“性能”日志记录级别

      可将包执行的日志记录级别设置为“性能”，以收集执行中每个组件的更详细持续时间信息。 在 [https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging) 中可以找到详细信息

  * 在 Azure 门户中 IR 监视页上检查 IR 节点性能。
    * 如何监视 SSIS Integration Runtime：[Azure-SSIS Integration Runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime)
    * Azure 门户上的数据工厂的“指标”中提供了 SSIS Integration Runtime 的 CPU/内存用量历史记录 ![监视 SSIS Integration Runtime 的指标](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)
