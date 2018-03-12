---
title: "Hadoop 安全性 - 已加入域的 HDInsight 群集 - Azure"
description: "了解..."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 10/31/2016
ms.date: 03/12/2018
ms.author: v-yiso
ms.openlocfilehash: ccf2e5f7e1b6dd024171b8d62d22a2f74f12f790
ms.sourcegitcommit: 34925f252c9d395020dc3697a205af52ac8188ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters"></a>已加入域的 HDInsight 群集的 Hadoop 安全性简介

Azure HDInsight 直到今天仅支持单个用户本地管理员。这很适合较小的应用程序团队或部门。 由于基于 Hadoop 的工作负荷在企业界更受欢迎，因此对企业级功能（如基于 active directory 的身份验证、多用户支持和基于角色的访问控制）的需求变得日益重要。 使用已加入域的 HDInsight 群集，可以创建一个已加入 Active Directory 域的 HDInsight 群集，配置可以通过 Azure Active Directory 进行身份验证并登录到 HDInsight 群集的企业员工列表。 企业外部的任何人都无法登录或访问 HDInsight 群集。 企业管理员可以使用 [Apache Ranger](http://hortonworks.com/apache/ranger/)配置基于角色的访问控制以确保 Hive 安全性，根据需要尽可能限制对数据的访问权限。 最后，管理员可以审核员工访问的数据以及对访问控制策略所做的任何更改，对公司资源进行高度控制。

> [!NOTE]
> 本文中所述的新功能仅在以下群集类型上可用：Hadoop、Spark 和交互式查询。 其他工作负荷，如 HBase、Storm 和 Kafka，会在以后的版本中启用这些功能。

> [!IMPORTANT]
> 在加入域的 HDInsight 上未启用 Oozie。

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

## <a name="benefits"></a>优点
企业安全性包含四大支柱 – 外围安全性、身份验证、授权和加密。

![已加入域的 HDInsight 群集可以稳固这些支柱](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png)。

### <a name="perimeter-security"></a>外围安全
HDInsight 中的外围安全性是使用虚拟网络和网关服务实现的。 现在，企业管理员可以在虚拟网络中创建 HDInsight 群集，使用网络安全组（入站或出站防火墙规则）来限制对虚拟网络的访问。 只有入站防火墙规则中定义的 IP 地址才能与 HDInsight 群集通信，从而实现了外围安全性。 另一个外围安全层是使用网关服务实现的。 网关是充当任何传入请求发送到 HDInsight 群集时的第一道防线的服务。 它接受请求，对它进行验证，并直到那时允许请求传递到群集中的其他节点，从而为群集中的其他名称节点和数据节点提供外围安全。

### <a name="authentication"></a>身份验证
企业管理员可以在[虚拟网络](/virtual-network/)中创建已加入域的 HDInsight 群集。 HDInsight 群集的节点将加入到由企业管理的域。 此过程是使用 [Azure Active Directory 域服务](../../active-directory-domain-services/active-directory-ds-overview.md)实现的。 群集中的所有节点都加入到企业管理的域。 通过此设置，企业员工可以使用其域凭据登录到群集节点。 他们还可以使用其域凭据向其他已批准的终结点（如 Hue、Ambari 视图、ODBC、JDBC、PowerShell 和 REST API）进行身份验证，以便与群集进行交互。 管理员可以完全控制并限制可以通过这些终结点与群集进行交互的用户数。

### <a name="authorization"></a>授权
大多数企业遵循的最佳做法是：并非每个员工都有权访问所有企业资源。 同样，使用此版本，管理员可以针对群集资源定义基于角色的访问控制策略。 例如，管理员可以配置 [Apache Ranger](http://hortonworks.com/apache/ranger/) ，为 Hive 设置访问控制策略。 此功能可确保员工能够仅根据需要访问尽可能多的数据，以在其工作中取得成功。 限制为仅管理员可以通过 SSH 访问群集。

### <a name="auditing"></a>审核
随着防止未经授权的用户访问 HDInsight 群集资源，确保数据安全，审核对群集资源的所有访问，数据是跟踪对资源的未经授权或无意识的访问所必需的。 管理员可以查看和报告对 HDInsight 群集资源与数据的所有访问。 管理员还可以查看并报告对 Apache Ranger 支持的终结点中完成的访问控制策略的所有更改。 已加入域的 HDInsight 群集使用熟悉的 Apache Ranger UI 搜索审核日志。 在后端，Ranger 使用 [Apache Solr](http://hortonworks.com/apache/solr/) 存储和搜索日志。

### <a name="encryption"></a>Encryption
保护数据对于满足组织安全性和合规性要求很重要，除了阻止未经授权的员工对数据进行访问外，还应通过对数据进行加密来保护数据。 HDInsight 群集的数据存储、Azure 存储 Blob 和 Azure Data Lake Storage 都支持在服务器端以透明方式进行静态[数据加密](../../storage/common/storage-service-encryption.md)。 HDInsight 安全群集能够与这种服务器端静态数据加密功能无缝协作。

## <a name="next-steps"></a>后续步骤
* 若要配置已加入域的 HDInsight 群集，请参阅 [Configure Domain-joined HDInsight clusters](apache-domain-joined-configure.md)（配置已加入域的 HDInsight 群集）。
* 若要管理已加入域的 HDInsight 群集，请参阅 [Configure Domain-joined HDInsight clusters](apache-domain-joined-manage.md)（管理已加入域的 HDInsight 群集）。
* 若要配置 Hive 策略和运行 Hive 查询，请参阅 [Configure Hive policies for Domain-joined HDInsight clusters](apache-domain-joined-run-hive.md)（为已加入域的 HDInsight 群集配置 Hive 策略）。
* 有关在已加入域的 HDInsight 群集上使用 SSH 运行 Hive 查询，请参阅[将 SSH 与 HDInsight 配合使用](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined)。
