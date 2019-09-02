---
title: 排查 Azure 数据工厂问题 | Microsoft Docs
description: 排查 Azure 数据工厂问题。 所有外部控制活动的通用文档。
services: data-factory
author: WenJason
manager: digimobile
ms.service: data-factory
ms.topic: troubleshooting
origin.date: 6/26/2019
ms.date: 08/12/2019
ms.author: v-jay
ms.reviewer: craigg
ms.openlocfilehash: 3f176608a19a347457f7e5d2742f948e34a0672a
ms.sourcegitcommit: 193f49f19c361ac6f49c59045c34da5797ed60ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2019
ms.locfileid: "68732454"
---
# <a name="troubleshooting-azure-data-factory"></a>排查 Azure 数据工厂问题
本文列出了常见的故障排除问题。

- [Azure Functions](#azure-functions)
- [自定义 (Azure Batch)](#custom-azure-batch)
- [HDInsight（Spark、Hive、MapReduce、Pig、Hadoop 流）](#hdinsight-spark-hive-mapreduce-pig-hadoop-streaming)
- [Web 活动](#web-activity)

## <a name="azure-functions"></a>Azure Functions

| 错误代码 | 错误消息                           | 说明                                                  | 可能的修复方法/建议的操作                           |
| ------------ | --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 3600         | 响应内容不是有效的 JObject | 这表示调用的 Azure 函数未在响应中返回 JSON 有效负载。 ADF Azure 函数活动仅支持 JSON 响应内容。 | 更新 Azure 函数以返回有效的 JSON 有效负载，例如，C# 函数可以返回 (ActionResult)new<OkObjectResult("{`\"Id\":\"123\"`}")。 |
| 3600         | 无效的 HttpMethod: ".."。               | 这表示活动有效负载中指定的 Http 方法不受 Azure 函数活动的支持。 | 支持的 Http 方法为：  <br/>PUT、POST、GET、DELETE、OPTIONS、HEAD、TRACE |



## <a name="custom-azure-batch"></a>自定义 (Azure Batch)
| 错误代码 | 错误消息                                                | 说明                                                  | 可能的修复方法/建议的操作                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2500         | 遇到意外的异常，且执行失败。             | 无法启动命令，或程序返回了错误代码。 | 检查该可执行文件是否存在。 如果程序已启动，请检查已上传到存储帐户的 stdout.txt 和 stderr.txt。 良好的做法是在代码中发出详细日志以进行调试。 |
| 2501         | 无法访问用户 Batch 帐户，请检查 Batch 帐户设置。 | 提供的 Batch 访问密钥或池名称不正确。            | 需要验证链接服务中的池名称和 Batch 访问密钥。 |
| 2502         | 无法访问用户存储帐户，请检查存储帐户设置。 | 提供的存储帐户名称或访问密钥不正确。       | 需要验证链接服务中的存储帐户名称和访问密钥。 |
| 2504         | 操作返回了无效的状态代码 'BadRequest'     | folderPath 中的文件过多（如果使用的是自定义活动）。  （resourceFiles 的总大小不能超过 32768 个字符。） | 删除不必要的文件，或压缩文件，并添加一个解压缩命令来解压缩文件，例如：powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;  $folder\yourProgram.exe |
| 2505         | 除非使用帐户密钥凭据，否则无法创建共享访问签名。 | 自定义活动仅支持使用访问密钥的存储帐户。 | 参考说明                                            |
| 2507         | 文件夹路径不存在或为空: ...            | 存储帐户的指定路径下没有文件。       | folderPath 必须包含要运行的可执行文件。 |
| 2508         | 资源文件夹中存在重复的文件。               | folderPath 的不同子文件夹中存在多个同名的文件。 | 自定义活动在 folderPath 下平展文件夹结构。  如果需要保留文件夹结构，请压缩文件，并使用一个解压缩命令将其解压缩到 Azure Batch 中，例如：powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;   $folder\yourProgram.exe |
| 2509         | Batch URL 无效，它必须采用 URI 格式。         | Batch URL 必须类似于 https:\//mybatchaccount.chinaeast.batch.azure.cn | 参考说明                                            |
| 2510         | 发送请求时出错。               | Batch URL 无效                                         | 验证 Batch URL。                                            |

## <a name="hdinsight-spark-hive-mapreduce-pig-hadoop-streaming"></a>HDInsight（Spark、Hive、MapReduce、Pig、Hadoop 流）

| 错误代码 | 错误消息                                                | 说明                                                  | 可能的修复方法/建议的操作                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2300、2310 | Hadoop 作业提交失败。 错误：无法解析远程名称”的问题。 <br/><br/>找不到群集。 | 提供的群集 URI 无效                              | 请确保未删除该群集，并且提供的 URI 正确。 可在任何浏览器中打开该 URI，应会看到 Ambari UI。 如果该群集在 vNet 中，则该 URI 应是专用 URI；你应尝试从属于同一 vNet 的 VM 打开它。 有关 [HDInsight 中的虚拟网络](/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services)的详细信息。 |
| 2300         | Hadoop 作业提交失败。 作业: ...，群集: .../。 错误：任务已取消。 | 作业提交已超时。                         | 这可能是普通的 HDInsight 连接问题或网络连接问题。 首先请确认可通过任何浏览器访问 HDInsight Ambari UI，并且你的凭据仍然有效。 如果使用自承载 IR，请确保从安装了自承载 IR 的 VM/计算机执行此操作。 然后再次尝试从 ADF 提交作业。 如果仍然失败，请联系 ADF 团队以获得支持。 |
| 2300         | 未授权: Ambari 用户名或密码不正确  <br/><br/>未授权: 用户 admin 在 Ambari 中被锁定   <br/><br/>403 - 禁止:访问被拒绝 | 为 HDInsight 提供的凭据不正确或已过期 | 请更正凭据，然后重新部署链接服务。 首先在任何浏览器中打开群集 URI 并尝试登录，确保凭据在 HDInsight 中有效。 如果凭据无效，可从 Azure 门户重置凭据。 |
| 2300、2310 | 502 - Web 服务器在充当网关或代理服务器时收到了无效响应       <br/>错误的网关 | 错误来自 HDInsight                               | 此错误来自 HDInsight 群集。 请参阅 [HDInsight 排除故障](https://hdinsight.github.io/ambari/ambari-ui-502-error.html)解决常见错误。    <br/>对于 Spark 群集，此错误也可能是[此原因](https://hdinsight.github.io/spark/spark-thriftserver-errors.html)造成的。 <br/><br/>[其他链接](/application-gateway/application-gateway-troubleshooting-502) |
| 2300         | Hadoop 作业提交失败。 作业: ...，群集: ...错误: {\"错误\":\"无法为提交作业请求提供服务，因为 templeton 服务正忙于处理过多的提交作业请求。 请等待一段时间，然后重试该操作。 请参考 config templeton.parallellism.job.submit 来配置并发请求。\  <br/><br/>Hadoop 作业提交失败。 作业:161da5d4-6fa8-4ef4-a240-6b6428c5ae2f，群集: https:\//abc-analytics-prod-hdi-hd-trax-prod01.azurehdinsight.cn/。   错误: {\"错误\":\"java.io.IOException: org.apache.hadoop.yarn.exceptions.YarnException:无法将 application_1561147195099_3730 提交到 YARN: org.apache.hadoop.security.AccessControlException:队列 root.joblauncher 已包含 500 个应用程序，无法接受应用程序提交: application_1561147195099_3730\ | 同时将过多的作业提交到 HDInsight | 请考虑限制提交到 HDI 的并发作业数。 如果这些作业是同一活动提交的，请参考 ADF 活动并发性。 更改触发器，将并发管道运行分散到不同的时间。 另请参阅 HDInsight 文档，根据错误中的建议调整“templeton.parallellism.job.submit”。 |
| 2303、2347 | Hadoop 作业失败并返回了退出代码 "5"。 有关更多详细信息，请参阅“wasbs://adfjobs@adftrialrun.blob.core.chinacloudapi.cn/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr”。  <br/><br/>Hive 执行失败并返回了错误代码 'UserErrorHiveOdbcCommandExecutionFailure'。   有关更多详细信息，请参阅“wasbs://adfjobs@eclsupplychainblobd.blob.core.chinacloudapi.cn/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out”。 | 作业已提交到 HDInsight，但在 HDInsight 上失败 | 已成功将作业提交到 HDInsight。 但该作业在群集上失败。 请在 HDInsight Ambari UI 中打开该作业，并打开日志，或者从存储中打开错误消息中指出的文件。错误详细信息将包含在该文件中。 |
| 2328         | 处理请求时发生内部服务器错误。 请重试请求或联系支持人员 | 此错误发生在按需 HDInsight 上。                              | 当 HDInsight 预配失败时，HDInsight 服务将发生此错误。 请联系 HDInsight 团队并向他们提供按需群集的名称。 |
| 2310         | java.lang.NullPointerException                               | 将作业提交到 Spark 群集时出错      | 此异常来自 HDInsight，其中隐蔽了实际问题。   请联系 HDInsight 团队获得支持，并向他们提供群集名称以及活动运行时间范围。 |
|              | 所有其他错误                                             |                                                              | 请参阅 [HDInsight 故障排除](../hdinsight/hdinsight-troubleshoot-guide.md)和 [HDInsight 常见问题解答](https://hdinsight.github.io/) |



## <a name="web-activity"></a>Web 活动

| 错误代码 | 错误消息                                                | 说明                                                  | 可能的修复方法/建议的操作                           |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2108         | 无效的 HttpMethod: ".."。                                    | 这表示活动有效负载中指定的 Http 方法不受 Web 活动的支持。 | 支持的 Http 方法为： <br/>PUT、POST、GET、DELETE |
| 2108         | 无效服务器错误 500                                     | 终结点上的内部错误                               | 检查 URL 的功能（使用 Fiddler/Postman）：[如何使用 Fiddler 创建 HTTP 会话](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 未授权 401                                             | 请求中缺少有效的身份验证方法                      | 提供有效的身份验证方法（令牌可能已过期）。   <br/><br/>检查 URL 的功能（使用 Fiddler/Postman）：[如何使用 Fiddler 创建 HTTP 会话](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 禁止 403                                                | 缺少必需的权限                                 | 检查用户对所要访问的资源的权限。   <br/><br/>检查 URL 的功能（使用 Fiddler/Postman）：[如何使用 Fiddler 创建 HTTP 会话](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 请求的错误 400                                              | 无效的 Http 请求                                         | 检查请求的 URL、谓词和正文。   <br/><br/>使用 Fiddler/Postman 验证请求：[如何使用 Fiddler 创建 HTTP 会话](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 未找到 404                                                | 未找到资源                                       | 使用 Fiddler/Postman 验证请求：[如何使用 Fiddler 创建 HTTP 会话](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 服务不可用                                          | 服务不可用                                       | 使用 Fiddler/Postman 验证请求：[如何使用 Fiddler 创建 HTTP 会话](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 不支持的媒体类型                                       | 内容类型与 Web 活动正文不匹配           | 指定与有效负载格式匹配的正确内容类型。使用 Fiddler/Postman 验证请求：[如何使用 Fiddler 创建 HTTP 会话](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 查找的资源已被删除、已更名或暂时不可用。 | 资源不可用                                | 使用 Fiddler/Postman 检查终结点：[如何使用 Fiddler 创建 HTTP 会话](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | 由于使用了无效的方法(HTTP 谓词)，无法显示你正在查找的页面。 | 在请求中指定了错误的 Web 活动方法   | 使用 Fiddler/Postman 检查终结点：[如何使用 Fiddler 创建 HTTP 会话](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |
| 2108         | invalid_payload                                              | Web 活动的正文不正确                       | 使用 Fiddler/Postman 检查终结点：[如何使用 Fiddler 创建 HTTP 会话](#how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application) |

#### <a name="how-to-use-fiddler-to-create-an-http-session-of-the-monitored-web-application"></a>如何使用 Fiddler 创建受监视 Web 应用程序的 HTTP 会话

1. 下载并安装 [Fiddler](https://www.telerik.com/download/fiddler)

2. 如果 Web 应用程序使用 HTTPS：

   1. 打开 Fiddler

   2. 转到“工具”>“Fiddler 选项”，并按以下屏幕截图所示进行选择。  

      ![fiddler-options](media/data-factory-troubleshoot-guide/fiddler-options.png)

3. 如果应用程序使用 SSL 证书，则还必须将 Fiddler 证书添加到设备。

4. 若要将 Fiddler 证书添加到设备，请转到“工具” > “Fiddler 选项” > “HTTPS” > “操作” > “将根证书导出到桌面”，以获取 Fiddler 证书。     

5. 关闭捕获，以便可以清除浏览器缓存来启动新的跟踪。

6. 1. 转到“文件” > “捕获流量”或按 **F12**。  
   2. 清除浏览器缓存以删除所有已缓存的项；必须重新下载这些项。

7. 创建请求： 

8. 1. 单击“编辑器”选项卡
   2. 设置 Http 方法和 URL
   3. 根据需要添加标头和请求正文
   4. 单击“执行”

9. 再次开始捕获流量，并在页面上完成相关的事务。

10. 完成后，转到“文件” > “保存” > “所有会话”。   

[此处](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)提供了有关 Fiddler 的详细信息

## <a name="next-steps"></a>后续步骤

如需查找问题的解决方案的更多帮助，下面是可以尝试的一些其他资源。

*  [MSDN 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home)



