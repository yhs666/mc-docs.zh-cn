---
title: 授权访问 Azure 存储 | Azure
description: 了解授权访问 Azure 存储的各种方式，包括共享密钥身份验证和共享访问签名。
services: storage
author: WenJason
manager: digimobile
ms.service: storage
ms.topic: article
origin.date: 05/18/2018
ms.date: 07/30/2018
ms.author: v-jay
ms.openlocfilehash: 76a36fa3f080daa8f56d4421204c9ae3e4121d02
ms.sourcegitcommit: 878351dae58cf32a658abcc07f607af5902c9dfa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/27/2018
ms.locfileid: "39295869"
---
# <a name="authorizing-access-to-azure-storage"></a>授权访问 Azure 存储

每次访问存储帐户中的数据时，客户端都会通过 HTTP/HTTPS 向 Azure 存储发出请求。 每个对安全资源的请求都必须经过授权，以便服务确保客户端具有访问数据所需的权限。 Azure 存储提供以下用于授权访问安全资源的选项：

- 用于 blob、文件、队列和表的**共享密钥授权**。 使用共享密钥的客户端会随使用存储帐户访问密钥签名的每个请求传递一个标头。 有关详细信息，请参阅[通过共享密钥进行授权](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/)。
- 用于 blob、文件、队列和表的**共享访问签名**。 共享访问签名 (SAS) 针对存储帐户中的资源提供有限的委托访问权限。 通过对签名的有效时间间隔或对它授予的权限添加约束，可灵活地管理访问权限。 有关详细信息，请参阅[使用共享访问签名 (SAS)](storage-dotnet-shared-access-signature-part-1.md)。
- 用于容器和 blob 的**匿名公共读取访问**。 无需授权。 有关详细信息，请参阅[管理对容器和 Blob 的匿名读取访问](../blobs/storage-manage-access-to-resources.md)。  

默认情况下，Azure 存储中的所有资源都受到保护，并且只能由帐户所有者使用。



