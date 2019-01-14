---
title: 将 HDFS CLI 与 Azure Data Lake Storage Gen2 预览版配合使用
description: 适用于 Azure Data Lake Storage Gen2 预览版的 HDFS CLI 简介
services: storage
author: WenJason
ms.service: storage
ms.topic: conceptual
origin.date: 12/06/2018
ms.date: 01/14/2019
ms.author: v-jay
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 9d4e81a6cec11a16656c16fb9080703220fa2762
ms.sourcegitcommit: 5eff40f2a66e71da3f8966289ab0161b059d0263
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2019
ms.locfileid: "54192948"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>将 HDFS CLI 与 Data Lake Storage Gen2 配合使用

使用 Azure Data Lake Storage Gen2 预览版，可以像使用 [Hadoop 分布式文件系统 (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html) 一样管理和访问数据。 无论你是附加 HDInsight 群集还是使用 Azure Databricks 运行 Apache Spark 作业来对 Azure 存储帐户中存储的数据执行分析，都可以使用命令行接口 (CLI) 来检索和操作所加载的数据。

## <a name="hdfs-cli-with-hdinsight"></a>将 HDFS CLI 与 HDInsight 配合使用

HDInsight 提供对在本地附加到计算节点的分布式文件系统的访问权限。 可以使用直接与 HDFS 以及与 Hadoop 支持的其他文件系统进行交互的 shell 来访问此文件系统。 下面是常用的命令和有用资源的链接。

>[!IMPORTANT]
>创建群集后便开始 HDInsight 群集计费，删除群集后停止计费。 HDInsight 群集按分钟收费，因此不再需要使用群集时，应将其删除。 若要了解如何删除群集，请参阅我们的[有关该主题的文章](../../hdinsight/hdinsight-delete-cluster.md)。 但是，即使删除了 HDInsight 群集，在启用了 Data Lake Storage Gen2 的存储帐户中存储的数据仍然会保留。

### <a name="get-a-list-of-files-or-directories"></a>获取文件或目录列表

    hdfs dfs -ls <args>

### <a name="create-a-directory"></a>创建目录

    hdfs dfs -mkdir [-p] <paths>

### <a name="delete-a-file-or-a-directory"></a>删除文件或目录

    hdfs dfs -rm [-skipTrash] URI [URI ...]

### <a name="use-the-hdfs-cli-with-an-hdinsight-hadoop-cluster-on-linux"></a>在 Linux 上结合使用 HDFS CLI 和 HDInsight Hadoop 群集

首先建立[对服务的远程访问](/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services)。 如果选择了 [SSH](/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)，则示例 PowerShell 代码将如下所示：

```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.cn
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```
可以在 Azure 门户中 HDInsight 群集边栏选项卡的“SSH + 群集登录”部分中找到连接字符串。 SSH 凭据是在创建群集时指定的。

有关 HDFS CLI 的详细信息，请参阅[官方文档](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html)和 [HDFS 权限指南](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)。

## <a name="set-file-and-directory-level-permissions"></a>设置文件和目录级别权限

设置并获取文件和目录级别访问权限。 以下命令可帮助你开始使用。 

若要详细了解 Azure Data Lake Gen2 文件系统的文件和目录级别权限，请参阅 [Azure Data Lake Storage Gen2 中的访问控制](data-lake-storage-access-control.md)。

### <a name="display-the-access-control-lists-acls-of-files-and-directories"></a>显示文件和目录的访问控制列表 (ACL)

    hdfs dfs -getfacl [-R] <path>

示例：

`hdfs dfs -getfacl -R /dir`

请参阅 [getfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#getfacl)

### <a name="set-acls-of-files-and-directories"></a>设置文件和目录的 ACL

    hdfs dfs -setfacl [-R] [-b|-k -m|-x <acl_spec> <path>]|[--set <acl_spec> <path>]

示例：

`hdfs dfs -setfacl -m user:hadoop:rw- /file`

请参阅 [setfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#setfacl)

### <a name="change-the-owner-of-files"></a>更改文件的所有者

    hdfs dfs -chown [-R] <new_owner>:<users_group> <URI>

请参阅 [chown](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chown)

### <a name="change-group-association-of-files"></a>更改文件的组关联

    hdfs dfs -chgrp [-R] <group> <URI>

请参阅 [chgrp](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chgrp)

### <a name="change-the-permissions-of-files"></a>更改文件的权限

    hdfs dfs -chmod [-R] <mode> <URI>

请参阅 [chmod](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chmod)

若要查看命令的完整列表，请访问 [Apache Hadoop 2.4.1 文件系统 Shell 指南](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html)网站。

