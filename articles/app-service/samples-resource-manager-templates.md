---
title: Azure 资源管理器模板示例 - 应用服务 | Azure
description: 应用服务的 Azure 资源管理器模板示例
services: app-service
documentationcenter: app-service
author: tfitzmac
tags: azure-service-management
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
origin.date: 01/04/2019
ms.date: 09/05/2019
ms.author: v-tawe
ms.custom: mvc
ms.openlocfilehash: a4bf4b2bb1a996fb823541e2a24781d7e4e558fc
ms.sourcegitcommit: bc34f62e6eef906fb59734dcc780e662a4d2b0a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2019
ms.locfileid: "70806895"
---
# <a name="azure-resource-manager-templates-for-app-service"></a>应用服务的 Azure 资源管理器模板

下表包含 Azure 应用服务的 Azure 资源管理器模板链接。 有关如何在创建应用模板时避免常见错误的建议，请参阅[有关使用 Azure 资源管理器模板部署应用的指南](deploy-resource-manager-template.md)。

| | |
|-|-|
|**部署应用**||
| [链接到 GitHub 存储库的应用](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| 部署从 GitHub 提取代码的应用服务应用。 |
| [使用自定义部署槽位的应用](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| 使用自定义部署槽位/环境部署应用服务应用。 |
|**配置应用**||
| [来自 Key Vault 的应用证书](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| 部署来自 Azure Key Vault 机密的应用服务应用证书并将其用于 SSL 绑定。 |
| [使用自定义域的应用](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain)| 使用自定义主机名部署应用服务应用。 |
| [使用自定义域和 SSL 的应用](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| 使用自定义主机名部署应用服务应用，并从 Key Vault 获取应用证书用于 SSL 绑定。 |
| [使用 GoLang 扩展的应用](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| 使用 Golang 站点扩展部署应用服务应用。 然后，可以在 Azure 中运行在 GoLang 上开发的 Web 应用程序。 |
| [使用 Java 8 和 Tomcat 8 的应用](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| 部署已启用 Java 8 和 Tomcat 8 的应用服务应用。 然后，可以在 Azure 中运行 Java 应用程序。 |
|**使用连接资源的应用**||
| [使用 MySQL 的应用](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| 在 Windows 上部署使用 Azure Database for MySQL 的应用服务应用。 |
| [使用 PostgreSQL 的应用](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| 在 Windows 上部署使用 Azure Database for PostgreSQL 的应用服务应用。 |
| [使用 SQL 数据库的应用](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| 在“基本”服务级别部署应用服务应用和 SQL 数据库。 |
| [使用 Blob 存储连接的应用](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| 使用 Azure Blob 存储连接字符串部署应用服务应用。 然后，可以从该应用使用 Blob 存储。 |
| [使用用于 Redis 的 Azure 缓存的应用](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| 使用用于 Redis 的 Azure 缓存部署应用服务应用。 |
| | |
