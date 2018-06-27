---
title: 快速入门 - 使用 Azure 门户创建 DNS 区域和记录
description: 通过本快速入门了解如何在 Azure DNS 中创建 DNS 区域和记录。 这是有关使用 Azure 门户创建和管理第一个 DNS 区域和记录的分步指南。
services: dns
documentationcenter: na
author: yunan2016
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 06/13/2018
ms.date: 06/25/2018
ms.author: v-nany
ms.openlocfilehash: c35272d42d7a09710cf7d4e496e4f191e2c830fc
ms.sourcegitcommit: 9f78ba87a377011f078025c56032b7f898d9742c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2018
ms.locfileid: "36299268"
---
# <a name="quickstart-configure-azure-dns-for-name-resolution-using-the-azure-portal"></a>快速入门：使用 Azure 门户配置用于名称解析的 Azure DNS

 可以将 Azure DNS 配置为在公共域中解析主机名称。 例如，如果你从某个域名注册机构购买了 contoso.com 域名，则可将 Azure DNS 配置为托管 contoso.com 域并将 www.contoso.com 解析为你的 Web 服务器或 Web 应用的 IP 地址。

在本快速入门中，请创建一个测试域，然后创建一个名为“www”的可以解析为 IP 地址 10.10.10.10 的地址记录。

必须说明的是，本快速入门中使用的所有名称和 IP 地址都只是示例，不代表实际方案。 当然，在适当情况下，也会讨论实际方案。

<!---
You can also perform these steps using [Azure PowerShell](dns-getstarted-powershell.md) or the cross-platform [Azure CLI 2.0](dns-getstarted-cli.md).
--->

DNS 区域用来存储某个特定域的 DNS 条目。 若要开始在 Azure DNS 中托管域，需要为该域名创建 DNS 区域。 随后会在此 DNS 区域内创建域的每个 DNS 条目（或记录）。 以下步骤说明如何执行此操作。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。

## <a name="create-a-dns-zone"></a>创建 DNS 区域

1. 登录到 Azure 门户。
2. 在左上角依次单击“+ 创建资源”、“网络”、“DNS 区域”，以便打开“创建 DNS 区域”页。

    ![DNS 区域](./media/dns-getstarted-portal/openzone650.png)

4. 在“创建 DNS 区域”页上，输入以下值，并单击“创建”：


   | **设置** | **值** | **详细信息** |
   |---|---|---|
   |**名称**|contoso.xyz|此示例的 DNS 区域的名称。 就本快速入门来说，可以根据需要使用任何值，只要该值尚未在 Azure DNS 服务器上配置。 实际值可以是从域名注册机构购买的域。|
   |**订阅**|[你的订阅]|选择要在其中创建 DNS 区域的订阅。|
   |**资源组**|**新建：** dns-test|创建资源组。 资源组名称在所选订阅中必须唯一。 |
   |**位置**|中国东部||

创建区域可能需要几分钟。

## <a name="create-a-dns-record"></a>创建 DNS 记录

现在创建新的地址记录（“A”记录）。 “A”记录用于将主机名解析为 IPv4 地址。

1. 在 Azure 门户的“收藏夹”窗格中单击“所有资源”。 在“所有资源”页中单击 **contoso.xyz** DNS 区域。 如果所选订阅中已包含多个资源，则可在“按名称筛选…”框中输入“contoso.xyz”， 轻松访问 DNS 区域。

1. 在“DNS 区域”页顶部，选择“+ 记录集”以打开“添加记录集”页。

1. 在“添加记录集”页中，输入以下值，并单击“确定”。 在此示例中，请创建一个“A”记录。

   |**设置** | **值** | **详细信息** |
   |---|---|---|
   |**名称**|www|记录的名称。 这是需用于主机的名称，而该主机则需解析为 IP 地址。|
   |**类型**|A| 要创建的 DNS 记录的类型。 “A”记录是最常用的，但也有其他记录类型，适合邮件服务器 (MX)、IPv6 地址 (AAAA) 等。 |
   |**TTL**|1|DNS 请求的生存时间。 指定 DNS 服务器和客户端可以将响应缓存多长时间。|
   |**TTL 单位**|小时|TTL 值的时间度量。|
   |**IP 地址**|10.10.10.10| 此值是将“A”记录解析后获得的 IP 地址。 这只是本快速入门的一个测试值。 对于实际示例，则请输入 Web 服务器的公共 IP 地址。|


由于在本快速入门中，你并没有实际购买真实的域名，因此不需将 Azure DNS 配置为在域名注册机构注册的名称服务器。 但在实际方案中，则需要 Internet 上的任何人都可以通过解析主机名连接到 Web 服务器或应用。 有关实际方案的详细信息，请参阅[将域委托给 Azure DNS](dns-delegate-domain-azure-dns.md)。


## <a name="test-the-name-resolution"></a>测试名称解析

有了测试区域且其中有了测试性的“A”记录以后，即可使用名为 nslookup 的工具测试名称解析。 

1. 首先，需记下要与 nslookup 配合使用的 Azure DNS 名称服务器。 

   区域的名称服务器列在 DNS 区域的“概览”页上。 复制其中一个名称服务器的名称：

   ![区域](./media/dns-getstarted-portal/viewzonens500.png)

2. 现在请打开命令提示符并运行以下命令：

   ```
   nslookup <host name> <name server>
   
   For example:

   nslookup www.contoso.xyz ns1-08.azure-dns.com
   ```

应该会看到类似于以下屏幕截图的内容：

![nslookup](media/dns-getstarted-portal/nslookup.PNG)

这用于验证名称解析是否可以正常使用。 www.contoso.xyz 解析为 10.10.10.10，就像所配置的那样！

## <a name="clean-up-resources"></a>清理资源

不再需要时，请删除 **dns-test** 资源组，以便删除在本快速入门中创建的资源。 为此，请单击 **dns-test** 资源组，然后单击“删除资源组”。


## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [在自定义域中为 web 应用创建 DNS 记录](./dns-web-sites-custom-domain.md)