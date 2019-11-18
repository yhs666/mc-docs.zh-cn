---
title: 排查 Azure 数据工厂问题 | Microsoft Docs
description: 了解如何排查 Azure 数据工厂中的外部控制活动问题。
services: data-factory
author: WenJason
ms.service: data-factory
ms.topic: troubleshooting
origin.date: 8/26/2019
ms.date: 11/11/2019
ms.author: v-jay
ms.reviewer: craigg
ms.openlocfilehash: 89b16afe99aa88b62e3eee5a91fb951854e43691
ms.sourcegitcommit: ff8dcf27bedb580fc1fcae013ae2ec28557f48ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2019
ms.locfileid: "73648769"
---
# <a name="troubleshoot-azure-data-factory"></a>排查 Azure 数据工厂问题

本文探讨 Azure 数据工厂中的外部控制活动的常用故障排除方法。

## <a name="connector-and-copy-activity"></a>连接器和复制活动

如果遇到连接器问题（例如，使用复制活动时遇到错误），请参阅[排查 Azure 数据工厂连接器问题](connector-troubleshoot-guide.md)。

## <a name="azure-functions"></a>Azure 函数

### <a name="error-code--3602"></a>错误代码：3602

- **消息**：`Invalid HttpMethod: {method}.`

- **原因：** 活动有效负载中指定的 Http 方法不受 Azure 函数活动的支持。 

- **建议**：支持的 Http 方法为 PUT、POST、GET、DELETE、OPTIONS、HEAD 和 TRACE。


### <a name="error-code--3603"></a>错误代码：3603

- **消息**：`Response content is not a valid JObject.`

- **原因：** 调用的 Azure 函数未在响应中返回 JSON 有效负载。 数据工厂中的 Azure 函数活动仅支持 JSON 响应内容。 

- **建议**：更新 Azure 函数以返回有效的 JSON 有效负载。 例如，C# 函数可以返回 `(ActionResult)new<OkObjectResult("{`\"Id\":\"123\"`}");`。


### <a name="error-code--3606"></a>错误代码：3606

- **消息**：`Azure function activity missing function key.`

- **原因：** Azure 函数活动定义不完整。 

- **建议**：请检查输入的 AzureFunction 活动 JSON 定义是否包含名为“functionKey”的属性。


### <a name="error-code--3607"></a>错误代码：3607

- **消息**：`Azure function activity missing function name.`

- **原因：** Azure 函数活动定义不完整。 

- **建议**：请检查输入的 AzureFunction 活动 JSON 定义是否包含名为“functionName”的属性。


### <a name="error-code--3608"></a>错误代码：3608

- **消息**：`Call to provided Azure function '{FunctionName}' failed with status-'{statusCode}' and message - '{message}'.` 

- **原因：** 活动定义中的 Azure 函数详细信息可能不正确。 

- **建议**：修复 Azure 函数详细信息，然后重试。


### <a name="error-code--3609"></a>错误代码：3609

- **消息**：`Azure function activity missing functionAppUrl.` 

- **原因：** Azure 函数活动定义不完整。 

- **建议**：请检查输入的 AzureFunction 活动 JSON 定义是否包含名为“functionAppUrl”的属性。


### <a name="error-code--3610"></a>错误代码：3610

- **消息**：`There was an error while calling endpoint.`

- **原因：** 函数 URL 可能不正确。

- **建议**：请确保活动 JSON 中的“functionAppUrl”值正确，然后重试。


### <a name="error-code--3611"></a>错误代码：3611

- **消息**：`Azure function activity missing Method in JSON.` 

- **原因：** Azure 函数活动定义不完整。

- **建议**：请检查输入的 AzureFunction 活动 JSON 定义是否包含名为“method”的属性。


### <a name="error-code--3612"></a>错误代码：3612

- **消息**：`Azure function activity missing LinkedService definition in JSON.`

- **原因：** Azure 函数活动定义可能不完整。

- **建议**：请检查输入的 AzureFunction 活动 JSON 定义是否包含链接服务详细信息。

## <a name="hdinsight"></a>HDInsight

下表适用于 Spark、Hive、MapReduce、Pig 和 Hadoop 流。


### <a name="error-code--2300"></a>错误代码：2300

- **消息**：`Hadoop job submission failed. Error: The remote name could not be resolved. <br/><br/>The cluster is not found.`

- **原因：** 提供的群集 URI 无效。 

- **建议**：请确保未删除该群集，并且提供的 URI 正确。 在浏览器中打开该 URI 时，应会看到 Ambari UI。 如果该群集位于虚拟网络中，则 URI 应是专用 URI。 若要打开它，请使用同一虚拟网络中的 VM。 有关详细信息，请参阅[直接连接到 Apache Hadoop 服务](/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services)。

<br/>

- **消息**：`Hadoop job submission failed. Job: …, Cluster: …/. Error: A task was canceled.`

- **原因：** 作业提交超时。 

- **建议**：这可能是普通的 HDInsight 连接问题或网络连接问题。 首先确认是否可以从任何浏览器打开 HDInsight Ambari UI。 确认凭据仍然有效。 如果使用自承载集成运行时 (IR)，请确保从安装了自承载 IR 的 VM 或计算机执行此操作。 然后再次尝试从数据工厂提交作业。 如果仍然失败，请联系数据工厂团队获得支持。


- **消息**：`Unauthorized: Ambari user name or password is incorrect  <br/><br/>Unauthorized: User admin is locked out in Ambari.   <br/><br/>403 - Forbidden: Access is denied.`

- **原因：** HDInsight 的凭据不正确或已过期。

- **建议**：请更正凭据，然后重新部署链接服务。 首先在任何浏览器中打开群集 URI 并尝试登录，确保凭据在 HDInsight 中有效。 如果凭据无效，可从 Azure 门户重置凭据。

<br/>

- **消息**：`502 - Web server received an invalid response while acting as a gateway or proxy server. <br/>Bad gateway.`

- **原因：** 此错误来自 HDInsight。

- **建议**：此错误来自 HDInsight 群集。 有关详细信息，请参阅 [Ambari UI 502 错误](https://hdinsight.github.io/ambari/ambari-ui-502-error.html)、[连接到 Spark Thrift 服务器时出现 502 错误](https://hdinsight.github.io/spark/spark-thriftserver-errors.html)、[连接到 Spark Thrift 服务器时出现 502 错误](https://hdinsight.github.io/spark/spark-thriftserver-errors.html)和[排查应用程序网关中的网关无效错误](/application-gateway/application-gateway-troubleshooting-502)。

<br/>

- **消息**：`Hadoop job submission failed. Job: …, Cluster: ... Error:   {\"error\":\"Unable to service the submit job request as   templeton service is busy with too many submit job requests. Please wait for some time before retrying the operation. Please refer to the config   templeton.parallellism.job.submit to configure concurrent requests. <br/><br/>Hadoop job submission failed. Job: xx, Cluster: name.   Error: {\"error\":\"java.io.IOException:   org.apache.hadoop.yarn.exceptions.YarnException: Failed to submit   application_1561147195099_3730 to YARN :   org.apache.hadoop.security.AccessControlException: Queue root.joblauncher already has 500 applications, cannot accept submission of application:   application_1561147195099_3730\`

- **原因：** 同时将过多的作业提交到 HDInsight。

- **建议**：请考虑限制提交到 HDInsight 的并发作业数。 如果这些作业是同一活动提交的，请参阅“数据工厂活动并发性”。 更改触发器，将并发管道运行分散到不同的时间。 请参阅 HDInsight 文档，根据错误中的建议调整 `templeton.parallellism.job.submit`。


### <a name="error-code--2303"></a>错误代码：2303

- **消息**：`Hadoop job failed with exit code '5'. See   'wasbs://adfjobs@xx.blob.core.chinacloudapi.cn/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr' for more details. <br/><br/>Hive execution failed with error code 'UserErrorHiveOdbcCommandExecutionFailure'.   See 'wasbs://adfjobs@xx.blob.core.chinacloudapi.cn/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out' for more details.`

- **原因：** 作业已提交到 HDInsight，但在 HDInsight 上失败。

- **建议**：已成功将作业提交到 HDInsight。 但该作业在群集上失败。 请在 HDInsight Ambari UI 中打开作业和日志，或者根据错误消息中的建议打开存储中的文件。 该文件会显示错误详细信息。


### <a name="error-code--2310"></a>错误代码：2310

- **消息**：`Hadoop job submission failed. Error: The remote name could not be resolved. <br/><br/>The cluster is not found.`

- **原因：** 提供的群集 URI 无效。 

- **建议**：请确保未删除该群集，并且提供的 URI 正确。 在浏览器中打开该 URI 时，应会看到 Ambari UI。 如果该群集位于虚拟网络中，则 URI 应是专用 URI。 若要打开它，请使用同一虚拟网络中的 VM。 有关详细信息，请参阅[直接连接到 Apache Hadoop 服务](/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services)。

<br/>

- **消息**：`502 - Web server received an invalid response while acting as a gateway or proxy server. <br/>Bad gateway.`

- **原因：** 此错误来自 HDInsight。

- **建议**：此错误来自 HDInsight 群集。 有关详细信息，请参阅 [Ambari UI 502 错误](https://hdinsight.github.io/ambari/ambari-ui-502-error.html)、[连接到 Spark Thrift 服务器时出现 502 错误](https://hdinsight.github.io/spark/spark-thriftserver-errors.html)、[连接到 Spark Thrift 服务器时出现 502 错误](https://hdinsight.github.io/spark/spark-thriftserver-errors.html)和[排查应用程序网关中的网关无效错误](/application-gateway/application-gateway-troubleshooting-502)。

<br/>

- **消息**：`java.lang.NullPointerException`

- **原因：** 此错误是在将作业提交到 Spark 群集时发生的。 

- **建议**：此异常来自 HDInsight。 它隐藏了实际问题。 请联系 HDInsight 团队获得支持。 请向支持人员提供群集名称和活动运行时间范围。


### <a name="error-code--2347"></a>错误代码：2347

- **消息**：`Hadoop job failed with exit code '5'. See 'wasbs://adfjobs@xx.blob.core.chinacloudapi.cn/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr' for more details. <br/><br/>Hive execution failed with error code 'UserErrorHiveOdbcCommandExecutionFailure'.   See 'wasbs://adfjobs@xx.blob.core.chinacloudapi.cn/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out' for more details.`

- **原因：** 作业已提交到 HDInsight，但在 HDInsight 上失败。

- **建议**：已成功将作业提交到 HDInsight。 但该作业在群集上失败。 请在 HDInsight Ambari UI 中打开作业和日志，或者根据错误消息中的建议打开存储中的文件。 该文件会显示错误详细信息。


### <a name="error-code--2328"></a>错误代码：2328

- **消息**：`Internal server error occurred while processing the request. Please retry the request or contact support. `

- **原因：** 此错误是在按需 HDInsight 中发生的。

- **建议**：当 HDInsight 预配失败时，HDInsight 服务将发生此错误。 请联系 HDInsight 团队并提供按需群集的名称。



## <a name="web-activity"></a>Web 活动

### <a name="error-code--2108"></a>错误代码：2108

- **消息**：`Invalid HttpMethod: '...'.`

- **原因：** Web 活动不支持活动有效负载中指定的 HTTP 方法。

- **建议**：支持的 HTTP 方法为 PUT、POST、GET 和 DELETE。

<br/>

- **消息**：`Invalid Server Error 500.`

- **原因：** 终结点上发生内部错误。

- **建议**：使用 Fiddler 或 Postman 检查 URL 的功能。

<br/>

- **消息**：`Unauthorized 401.`

- **原因：** 请求中缺少有效的身份验证方法。

- **建议**：令牌可能已过期。 提供有效的身份验证方法。 使用 Fiddler 或 Postman 检查 URL 的功能。

<br/>

- **消息**：`Forbidden 403.`

- **原因：** 缺少必需的权限。

- **建议**：检查用户对所要访问的资源的权限。 使用 Fiddler 或 Postman 检查 URL 的功能。

<br/>

- **消息**：`Bad Request 400.`

- **原因：** 无效的 HTTP 请求。

- **建议**： 检查请求的 URL、谓词和正文。 使用 Fiddler 或 Postman 验证请求。

<br/>

- **消息**：`Not found 404.` 

- **原因：** 找不到资源。   

- **建议**：使用 Fiddler 或 Postman 验证请求。

<br/>

- **消息**：`Service unavailable.`

- **原因：** 服务不可用。

- **建议**：使用 Fiddler 或 Postman 验证请求。

<br/>

- **消息**：`Unsupported Media Type.`

- **原因：** 内容类型与 Web 活动正文不匹配。

- **建议**：指定与有效负载格式匹配的内容类型。 使用 Fiddler 或 Postman 验证请求。

<br/>

- **消息**：`The resource you are looking for has been removed, has had its name changed, or is temporarily unavailable.`

- **原因：** 资源不可用。 

- **建议**：使用 Fiddler 或 Postman 检查终结点。

<br/>

- **消息**：`The page you are looking for cannot be displayed because an invalid method (HTTP verb) is being used.`

- **原因：** 在请求中指定了错误的 Web 活动方法。

- **建议**：使用 Fiddler 或 Postman 检查终结点。

<br/>

- **消息**：`invalid_payload`

- **原因：** Web 活动正文不正确。

- **建议**：使用 Fiddler 或 Postman 检查终结点。


### <a name="error-code--2208"></a>错误代码：2208

- **消息**：`Invoking Web Activity failed with HttpStatusCode - {0}.`

- **原因：** 目标服务返回了失败状态。

- **建议**：使用 Fiddler/Postman 验证请求。


### <a name="error-code--2308"></a>错误代码：2308

- **消息**：`No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **原因：** 此错误的原因有很多，例如，与网络连接、DNS 故障、服务器证书验证或超时有关。

- **建议**：使用 Fiddler/Postman 验证请求。


若要使用 Fiddler 创建受监视 Web 应用程序的 HTTP 会话：

1. 下载、安装并打开 [Fiddler](https://www.telerik.com/download/fiddler)。

1. 如果 Web 应用程序使用 HTTPS，请转到“工具” > “Fiddler 选项” > “HTTPS”。    选择“捕获 HTTPS 连接”和“解密 HTTPS 流量”。   
   
   ![Fiddler 选项](media/data-factory-troubleshoot-guide/fiddler-options.png)

1. 如果应用程序使用 SSL 证书，请将 Fiddler 证书添加到设备。 转到“工具” > “Fiddler 选项” > “HTTPS” > “操作” > “将根证书导出到桌面”。     

1. 转到“文件” > “捕获流量”来关闭捕获。   或者按 **F12**。

1. 清除浏览器缓存以删除所有已缓存的项；必须重新下载这些项。

1. 创建请求： 

   a. 选择“编辑器”选项卡。 

   b. 设置 HTTP 方法和 URL。

   c. 根据需要添加标头和请求正文。

   d. 选择“执行”  。

9. 再次打开流量捕获，并在页面上完成相关的事务。

10. 转到“文件” > “保存” > “所有会话”。   

有关详细信息，请参阅 [Fiddler入门](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)。

## <a name="next-steps"></a>后续步骤

尝试通过以下资源获得故障排除方面的更多帮助：

*  [MSDN 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home)



