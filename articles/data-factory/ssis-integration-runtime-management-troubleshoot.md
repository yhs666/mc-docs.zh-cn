---
title: 在 Azure 数据工厂中排查 SSIS Integration Runtime 管理问题 | Microsoft Docs
description: 本文提供有关排查 SSIS Integration Runtime (SSIS IR) 管理问题的指导
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 07/08/2019
ms.date: 08/12/2019
author: WenJason
ms.author: v-jay
ms.reviewer: sawinark
manager: digimobile
ms.openlocfilehash: b8bdfcc4e01c5a24c738c2fbe6e72a849b9557b1
ms.sourcegitcommit: 871688d27d7b1a7905af019e14e904fabef8b03d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68908736"
---
# <a name="troubleshoot-ssis-integration-runtime-management-in-azure-data-factory"></a>在 Azure 数据工厂中排查 SSIS Integration Runtime 管理问题

本文档提供有关排查 SSIS Integration Runtime (SSIS IR) 管理问题的指导。

## <a name="overview"></a>概述

如果预配或取消预配 SSIS IR 时出现任何问题，ADF 门户中会显示错误消息，或者 PowerShell cmdlet 会返回错误消息。 错误始终以错误代码的格式显示，并附带详细的错误消息。

如果错误代码为 InternalServerError，则表示服务出现了暂时性的问题。 可以稍后重试操作。 如果重试不起作用，请联系 Azure 数据工厂支持团队。

有三个主要的外部依赖项可能会导致错误：Azure SQL 数据库服务器或托管实例、自定义安装脚本和虚拟网络配置（如果错误代码不是 InternalServerError）。

## <a name="azure-sql-database-server-or-managed-instance-issues"></a>Azure SQL 数据库服务器或托管实例问题

使用 SSIS 目录数据库预配 SSIS IR 时需要 Azure SQL 数据库服务器或托管实例。 SSIS IR 应可访问该 Azure SQL 数据库服务器或托管实例。 Azure SQL 数据库服务器或托管实例的帐户应有权创建 SSIS 目录数据库 (SSISDB)。 如果出现任何错误，ADF 门户中会显示错误代码以及详细的 SQL 异常消息。 请遵循以下步骤根据错误代码进行故障排除。

### <a name="azuresqlconnectionfailure"></a>AzureSqlConnectionFailure

预配新的 SSIS IR 时或运行 IR 期间，可能会出现此问题。

如果在预配 IR 期间出现该错误，则原因可能如下。可以在错误消息中获取详细的 SqlException 消息。

* 网络连接问题。 检查 SQL Server 或托管实例主机名是否可访问，并且没有任何防火墙或 NSG 阻止 SSIS IR 访问服务器。
* 在使用 SQL 身份验证的情况下登录失败。 这表示提供的帐户无法登录到 SQL Server。 请确保提供正确的用户帐户。
* 在使用 AAD 身份验证（托管标识）的情况下登录失败。 将工厂的托管标识添加到 AAD 组，使该托管标识有权访问目录数据库服务器。
* 连接超时。此错误始终由安全相关的配置造成。 建议创建新的 VM，使 VM 加入 IR 所在的同一 VNet（如果 IR 在 VNet 中），然后安装 SSMS 并检查 Azure SQL 数据库服务器或托管实例的状态。

对于其他问题，请参阅详细的 SQL 异常错误消息，并解决错误消息中显示的问题。 如果仍然遇到问题，请联系 Azure SQL 数据库服务器或托管实例支持团队。

如果在运行 IR 期间出现错误，原因可能是网络安全组或防火墙发生了一些更改，导致 SSIS IR 工作器节点不再能够访问 Azure SQL 数据库服务器或托管实例。 取消阻止 SSIS IR 工作器节点访问 Azure SQL 数据库服务器或托管实例。

### <a name="catalogcapacitylimiterror"></a>CatalogCapacityLimitError

错误消息类似于“数据库 'SSISDB' 已达到其大小配额。 请将数据分区或删除、删除索引，或查阅文档以找到可能的解决方法。” 可能的解决方法包括：
* 提高 SSISDB 的大小配额。
* 更改 SSISDB 的配置以减小大小，例如：
   * 缩短保留期，减少项目版本数。
   * 缩短日志的保留期。
   * 更改日志的默认级别，等等。

### <a name="catalogdbbelongstoanotherir"></a>CatalogDbBelongsToAnotherIR

此错误表示 Azure SQL 数据库服务器或托管实例中已创建一个由其他 IR 使用的 SSISDB。 需要提供不同的 Azure SQL 数据库服务器或托管实例，或者删除现有 SSISDB 并重启新的 IR。

### <a name="catalogdbcreationfailure"></a>CatalogDbCreationFailure

此错误可能由以下原因导致：

* 为 SSIS IR 配置的用户帐户无权创建数据库。 可向该用户授予创建数据库的权限。
* 创建数据库超时，例如执行超时、数据库操作超时，等等。 可以稍后重试，以查看问题是否已解决。 如果重试不起作用，请联系 Azure SQL 数据库服务器或托管实例支持团队。

对于其他问题，请查看 SQL 异常错误消息，并解决错误消息中提到的问题。 如果仍然遇到问题，请联系 Azure SQL 数据库服务器或托管实例支持团队。

### <a name="invalidcatalogdb"></a>InvalidCatalogDb

错误消息类似于“对象名称 'catalog.catalog_properties' 无效”，这表示你已有一个名为 SSISDB 的数据库，但该数据库不是由 SSIS IR 创建，或者上次预配 SSIS IR 时出现的错误导致该数据库处于无效状态。 可以删除名为 SSISDB 的现有数据库，或者为 IR 配置新的 Azure SQL 数据库服务器或托管实例。

## <a name="custom-setup"></a>自定义安装

自定义安装提供一个界面，用于在预配或重新配置 SSIS IR 期间添加你自己的设置步骤。 有关详细信息，请参阅 [Azure-SSIS Integration Runtime 的自定义安装](/data-factory/how-to-configure-azure-ssis-ir-custom-setup)。

确保容器中只包含必要的自定义安装文件，因为容器中的所有文件将下载到 SSIS IR 工作器节点。 建议在本地计算机上测试自定义安装脚本并解决任何脚本执行问题，然后在 SSIS IR 中运行该脚本。

在运行 IR 期间也会检查自定义安装脚本容器，因为 SSIS IR 会定期更新，这需要再次访问容器以下载自定义安装脚本并再次安装。 检查项目包括容器是否可访问，以及 main.cmd 文件是否存在。

如果使用自定义安装时出现任何错误，你将看到代码为 CustomSetupScriptFailure 的错误。请检查具有附属错误代码的错误消息。  请遵循以下步骤根据附属错误代码进行故障排除。  

### <a name="customsetupscriptblobcontainerinaccessible"></a>CustomSetupScriptBlobContainerInaccessible

这表示 SSIS IR 无法访问自定义安装的 Azure Blob 容器。 检查容器的 SAS URI 是否可访问且未过期。

如果 IR 处于运行中状态，请先停止 IR，使用新的自定义安装容器 SAS URI 重新配置 IR，然后再次启动 IR。

### <a name="customsetupscriptnotfound"></a>CustomSetupScriptNotFound

这表示 SSIS IR 在 Blob 容器中找不到自定义安装脚本 (main.cmd)。 确保容器中存在 main.cmd（自定义安装程序的入口点）。

### <a name="customsetupscriptexecutionfailure"></a>CustomSetupScriptExecutionFailure

这表示执行自定义安装脚本 (main.cmd) 失败。可以先在本地计算机上尝试执行该脚本，或者检查 Blob 容器中的自定义安装执行日志。

### <a name="customsetupscripttimeout"></a>CustomSetupScriptTimeout

这表示执行自定义安装脚本超时。 确保 Blob 容器只包含必要的自定义安装文件。 还可以检查 Blob 容器中的自定义安装执行日志。 自定义安装的最大期限设置为 45 分钟（45 分钟后将会超时），这包括从容器下载所有文件并将其安装在 Azure-SSIS IR 上的时间。 如果需要延长期限，请提交支持票证。

### <a name="customsetupscriptloguploadfailure"></a>CustomSetupScriptLogUploadFailure

这表示将自定义安装执行日志上传到 Blob 容器失败，原因是 SSIS IR 对 Blob 容器没有写入权限，或者存储或网络出现了一些问题。 如果自定义安装成功，则此错误不会影响任何 SSIS 功能，但日志会缺失。 如果自定义安装失败并出现另一种错误，并且我们无法上传日志，则我们首先会报告此错误以便可以上传日志进行分析，解决此问题后，我们将报告其他指定的问题。 如果重试后未解决此问题，请联系 Azure 数据工厂支持团队。

## <a name="virtual-network-configuration"></a>虚拟网络配置

将 SSIS IR 加入虚拟网络 (VNet) 时，SSIS IR 将使用用户订阅下的 VNet。 有关详细信息，请参阅[将 Azure-SSIS Integration Runtime 加入虚拟网络](/data-factory/join-azure-ssis-integration-runtime-virtual-network)。

出现 VNet 相关的问题时，会显示如下错误

### <a name="invalidvnetconfiguration"></a>InvalidVnetConfiguration

此错误可能由多种不同的原因造成。 请遵循以下步骤根据附属错误代码进行故障排除。

### <a name="forbidden"></a>禁止

错误消息类似于“未为当前帐户启用 subnetId。 Microsoft.Batch 资源提供程序未在 VNet 的同一订阅下注册。”

这表示 Azure Batch 无法访问你的 VNet。 请在 VNet 的同一订阅下注册 Microsoft.Batch 资源提供程序。

### <a name="invalidpropertyvalue"></a>InvalidPropertyValue

错误消息类似于“指定的 VNet 不存在，或者 Batch 服务无权访问它”或“指定的子网 xxx 不存在”。

这表示 VNet 不存在，或 Azure Batch 服务无法访问它，或提供的子网不存在。 确保 VNet 和子网存在，并且 Azure Batch 可以访问它们。

### <a name="misconfigureddnsserverornsgsettings"></a>MisconfiguredDnsServerOrNsgSettings

消息类似于“无法在 VNet 中预配 Integration Runtime。 如果配置了 DNS 服务器或 NSG 设置，请确保 DNS 服务器可访问，并且 NSG 配置正确”

可能是你对 DNS 服务器或 NSG 设置使用了某种自定义配置，导致 SSIS IR 所需的 Azure 服务器名称无法解析或不可访问。 有关详细信息，请参阅 [SSIS IR VNet 配置](/data-factory/join-azure-ssis-integration-runtime-virtual-network)文档。 如果仍然遇到问题，请联系 Azure 数据工厂支持团队。

### <a name="vnetresourcegrouplockedduringupgrade"></a>VNetResourceGroupLockedDuringUpgrade

SSIS IR 将定期自动更新，在升级过程中会创建新的 Azure Batch 池，删除旧的 Azure Batch 池，删除旧池的 VNet 相关资源，并在订阅下创建新的 VNet 相关资源。 此错误表示由于存在订阅或资源组级别的删除锁，因此删除旧池的 VNet 相关资源失败。 请帮助删除删除锁。

### <a name="vnetresourcegrouplockedduringstart"></a>VNetResourceGroupLockedDuringStart

SSIS IR 预配可能因某种原因而失败，如果发生失败，创建的所有资源将被删除。 但是，由于存在订阅或资源组级别的资源删除锁，删除 VNet 资源失败。 删除删除锁并重启 IR。

### <a name="vnetresourcegrouplockedduringstop"></a>VNetResourceGroupLockedDuringStop

停止 SSIS IR 时，将删除与 VNet 相关的所有资源，但由于存在订阅或资源组级别的资源删除锁，因此删除操作失败。 请帮助删除删除锁并重试停止。

### <a name="nodeunavailable"></a>NodeUnavailable

此错误是在运行 IR 期间发生的，表示 IR 过去正常，但现在不正常，原因始终是 DNS 服务器或 NSG 配置发生更改，导致 SSIS IR 无法连接到依赖的服务。请帮助解决 DNS 服务器或 NSG 配置问题。有关详细信息，请参阅 [SSIS IR VNet 配置](/data-factory/join-azure-ssis-integration-runtime-virtual-network)。 如果仍然遇到问题，请联系 Azure 数据工厂支持团队。
