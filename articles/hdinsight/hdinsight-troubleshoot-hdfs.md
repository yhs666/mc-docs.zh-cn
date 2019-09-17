---
title: Azure HDInsight 中的 HDFS 故障排除
description: 获取有关使用 HDFS 和 Azure HDInsight 的常见问题答案。
services: hdinsight
author: hrasheed-msft
ms.assetid: 4C33828F-2982-47F0-B858-C32FFF634D9E
ms.service: hdinsight
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 08/14/2019
ms.date: 09/16/2019
ms.author: v-yiso
ms.openlocfilehash: 2a69cd31e26bdd29087eadb0efc4bd12b7cb3bd4
ms.sourcegitcommit: dd0ff08835dd3f8db3cc55301815ad69ff472b13
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70736664"
---
# <a name="troubleshoot-apache-hadoop-hdfs-by-using-azure-hdinsight"></a>使用 Azure HDInsight 对 Apache Hadoop HDFS 进行故障排除

了解在 Apache Ambari 中使用 Hadoop 分布式文件系统 (HDFS) 有效负载时遇到的常见问题及其解决方法。

## <a name="how-do-i-access-local-hdfs-from-inside-a-cluster"></a>如何从群集内访问本地 HDFS？

### <a name="issue"></a>问题

从 HDInsight 群集内通过命令行和应用程序代码（而不是 Azure Blob 存储）访问本地 HDFS。   

### <a name="resolution-steps"></a>解决步骤

1. 在命令提示符下，按原义使用 `hdfs dfs -D "fs.default.name=hdfs://mycluster/" ...`，如以下命令中所示：

    ```output
    hdfs dfs -D "fs.default.name=hdfs://mycluster/" -ls /
    Found 3 items
    drwxr-xr-x   - hdiuser hdfs          0 2017-03-24 14:12 /EventCheckpoint-30-8-24-11102016-01
    drwx-wx-wx   - hive    hdfs          0 2016-11-10 18:42 /tmp
    drwx------   - hdiuser hdfs          0 2016-11-10 22:22 /user
    ```

2. 从源代码按原义使用 URI `hdfs://mycluster/`，如以下示例应用程序中所示：

    ```Java
    import java.io.IOException;
    import java.net.URI;
    import org.apache.commons.io.IOUtils;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.*;
    
    public class JavaUnitTests {
    
        public static void main(String[] args) throws Exception {
    
            Configuration conf = new Configuration();
            String hdfsUri = "hdfs://mycluster/";
            conf.set("fs.defaultFS", hdfsUri);
            FileSystem fileSystem = FileSystem.get(URI.create(hdfsUri), conf);
            RemoteIterator<LocatedFileStatus> fileStatusIterator = fileSystem.listFiles(new Path("/tmp"), true);
            while(fileStatusIterator.hasNext()) {
                System.out.println(fileStatusIterator.next().getPath().toString());
            }
        }
    }
    ```
3. 使用以下命令在 HDInsight 群集上运行已编译的 .jar 文件（例如，名为 `java-unit-tests-1.0.jar` 的文件）：

    ```apache
    hadoop jar java-unit-tests-1.0.jar JavaUnitTests
    hdfs://mycluster/tmp/hive/hive/5d9cf301-2503-48c7-9963-923fb5ef79a7/inuse.info
    hdfs://mycluster/tmp/hive/hive/5d9cf301-2503-48c7-9963-923fb5ef79a7/inuse.lck
    hdfs://mycluster/tmp/hive/hive/a0be04ea-ae01-4cc4-b56d-f263baf2e314/inuse.info
    hdfs://mycluster/tmp/hive/hive/a0be04ea-ae01-4cc4-b56d-f263baf2e314/inuse.lck
    ```

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：

* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。
