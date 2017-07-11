---
title: "将现有的自定义 DNS 名称映射到 Azure Web 应用 | Azure"
description: "了解如何在 Azure 应用服务中向 Web 应用、移动应用后端或 API 应用添加现有的自定义 DNS 域名（虚域）。"
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
origin.date: 05/04/2017
ms.date: 07/03/2017
ms.author: v-dazen
ms.custom: mvc
ms.openlocfilehash: 5073b9c0e2a6010b621f9cbd64d315148b0f0296
ms.sourcegitcommit: b1d2bd71aaff7020dfb3f7874799e03df3657cd4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2017
---
<a id="map-an-existing-custom-dns-name-to-azure-web-apps" class="xliff"></a>

# 将现有的自定义 DNS 名称映射到 Azure Web 应用

[Azure Web 应用](app-service-web-overview.md)提供高度可缩放、自修补的 Web 托管服务。 本教程介绍如何将现有的自定义 DNS 名称映射到 Azure Web 应用。

![在门户中导航到 Azure 应用](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 使用 CNAME 记录映射子域（例如 `www.contoso.com`）
> * 使用 A 记录映射根域（例如 `contoso.com`）
> * 使用 CNAME 记录映射通配符域（例如 `*.contoso.com`）
> * 使用脚本自动执行域映射

可以使用 **CNAME 记录**或 **A 记录**将自定义 DNS 名称映射到应用服务。 

> [!NOTE]
> 我们建议对除根域（例如 `contoso.com`）以外的所有自定义 DNS 名称使用 CNAME。 

<a id="prerequisites" class="xliff"></a>

## 先决条件

若要完成本教程，需执行以下操作：

* [创建一个应用服务应用](/app-service/)，或者使用为其他教程创建的应用。
* 购买一个域名并确保你对你的域提供商的 DNS 注册表具有访问权限。

  例如，若要添加 `contoso.com` 和 `www.contoso.com` 的 DNS 条目，必须能够配置 `contoso.com` 根域的 DNS 设置。

<a id="prepare-the-app" class="xliff"></a>

## 准备应用

若要将自定义 DNS 名称映射到 Web 应用，Web 应用的[应用服务计划](https://www.azure.cn/pricing/details/app-service/)必须位于付费层（“共享”、“基本”、“标准”或“高级”）。 在此步骤中，需确保应用服务计划位于受支持的定价层。

<a id="sign-in-to-azure" class="xliff"></a>

### 登录 Azure

打开 [Azure 门户](https://portal.azure.cn)，然后使用 Azure 帐户登录。

<a id="navigate-to-the-app-in-the-azure-portal" class="xliff"></a>

### 在 Azure 门户中导航到应用

从左侧菜单中选择“应用服务”，然后选择应用的名称。

![在门户中导航到 Azure 应用](./media/app-service-web-tutorial-custom-domain/select-app.png)

可以看到应用服务应用的管理页面。  

<a id="check-the-pricing-tier" class="xliff"></a>

### 检查定价层

在应用页面的左侧导航窗格中，向下滚动到“设置”部分，然后选择“扩展(应用服务计划)”。

![扩展菜单](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

蓝色的框突出显示了应用的当前层。 检查以确保应用不在“免费”层中。 **免费**层不支持自定义 DNS。 

![检查定价层](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

如果应用服务计划不是**免费的**，请关闭“选择定价层”页面并跳到[映射 CNAME 记录](#cname)。

<a id="scale-up-the-app-service-plan" class="xliff"></a>

### 扩展应用服务计划

选择任一非免费层（“共享”、“基本”、“标准”或“高级”）。 

单击“选择”。

![检查定价层](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

如果看到以下通知，则表示缩放操作已完成。

![缩放操作确认](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

<a id="map-a-cname-record" class="xliff"></a>

## 映射 CNAME 记录

在教程示例中，将为 `www` 子域（例如 `www.contoso.com`）添加 CNAME 记录。

<a id="access-dns-records-with-domain-provider" class="xliff"></a>

### 通过域提供商访问 DNS 记录

登录到域提供商的网站。

查找管理 DNS 记录的页面。 每个域提供商都有其自己的 DNS 记录界面，因此应查阅提供商的文档。 查找站点中标签为“域名”、“DNS”或“名称服务器管理”的链接或区域。 

通常，通过查看帐户信息，然后查找如“我的域”之类的链接，便可以找到 DNS 记录管理页面。 转到此页面，然后查找名为**区域文件**、**DNS 记录**或**高级配置**等名称的链接。

以下屏幕截图是 DNS 记录管理页面的一个示例：

![示例 DNS 记录页](./media/app-service-web-tutorial-custom-domain/example-record-ui.png)

在示例屏幕截图中，选择“添加”来创建记录。 某些提供商提供了不同的链接来添加不同的记录类型。 同样，需要查阅提供商的文档。

> [!NOTE]
> 对于某些提供商，在选择单独的“保存更改”链接之前，这些 DNS 记录不会生效。 

<a id="create-the-cname-record" class="xliff"></a>

### 创建 CNAME 记录

添加一条 CNAME 记录来将子域映射到应用的默认主机名 (`<app_name>.chinacloudsites.cn`)。

对于 `www.contoso.com` 域示例，添加一条 CNAME 记录来将名称 `www` 映射到 `<app_name>.chinacloudsites.cn`。

在添加 CNAME 后，DNS 记录页面看起来如以下示例：

![在门户中导航到 Azure 应用](./media/app-service-web-tutorial-custom-domain/cname-record.png)

<a id="enable-the-cname-record-mapping-in-azure" class="xliff"></a>

### 在 Azure 中启用 CNAME 记录映射

在 Azure 门户中，在应用页面的左侧导航窗格中，选择“自定义域”。 

![自定义域菜单](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

在应用的“自定义域”页面中，将完全限定的自定义 DNS 名称 (`www.contoso.com`) 添加到列表。

单击“添加主机名”旁边的 **+** 图标。

![添加主机名](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

键入你为其添加了 CNAME 记录的完全限定的域名，例如 `www.contoso.com`。 

选择“验证”。

“添加主机名”按钮会被激活。 

确保“主机名记录类型”设置为“CNAME（www.example.com 或任何子域）”。

选择“添加主机名”。

![将 DNS 名称添加到应用](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

新主机名可能需要经过一段时间后才会反映在应用的“自定义域”页中。 请尝试刷新浏览器来更新数据。

![已添加 CNAME 记录](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

如果之前错过了某个步骤或者在某个位置的输入不正确，则会在页面的底部看到验证错误。

![验证错误](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

<a id="map-an-a-record" class="xliff"></a>

## 映射 A 记录

在教程示例中，将为根域（例如 `contoso.com`）添加 A 记录。 

<a name="info"></a>

<a id="copy-the-apps-ip-address" class="xliff"></a>

### 复制应用的 IP 地址

若要映射 A 记录，需要具有应用的外部 IP 地址。 在 Azure 门户中，可以在应用的“自定义域”页面中找到此 IP 地址。

在 Azure 门户中，在应用页面的左侧导航窗格中，选择“自定义域”。 

![自定义域菜单](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

在“自定义域”页面中，复制应用的 IP 地址。

![在门户中导航到 Azure 应用](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

<a id="access-dns-records-with-domain-provider" class="xliff"></a>

### 通过域提供商访问 DNS 记录

登录到域提供商的网站。

查找管理 DNS 记录的页面。 每个域提供商都有其自己的 DNS 记录界面，因此应查阅提供商的文档。 查找站点中标签为“域名”、“DNS”或“名称服务器管理”的链接或区域。 

通常，通过查看帐户信息，然后查找如“我的域”之类的链接，便可以找到 DNS 记录管理页面。 转到此页面，然后查找名为**区域文件**、**DNS 记录**或**高级配置**等名称的链接。

以下屏幕截图是 DNS 记录管理页面的一个示例：

![示例 DNS 记录页](./media/app-service-web-tutorial-custom-domain/example-record-ui.png)

在示例屏幕截图中，选择“添加”来创建记录。 某些提供商提供了不同的链接来添加不同的记录类型。 同样，需要查阅提供商的文档。

> [!NOTE]
> 对于某些提供商，在选择单独的“保存更改”链接之前，这些 DNS 记录不会生效。 

<a id="create-the-a-record" class="xliff"></a>

### 创建 A 记录

若要将 A 记录映射到应用，应用服务需要**两条** DNS 记录：

- **A** 记录映射到应用的 IP 地址。
- **TXT** 记录映射到应用的默认主机名 `<app_name>.chinacloudsites.cn`。 此记录使得应用服务可以验证你是否拥有要映射的自定义域。

对于 `www.contoso.com` 域示例，根据下表创建 A 和 TXT 记录（`@` 通常表示根域）。 

| 记录类型 | 主机 | 值 |
| - | - | - |
| A | `@` | 通过[复制应用的 IP 地址](#info)获得的 IP 地址 |
| TXT | `@` | `<app_name>.chinacloudsites.cn` |

添加记录后，DNS 记录页面看起来如以下示例：

![DNS 记录页](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

<a id="enable-the-a-record-mapping-in-the-app" class="xliff"></a>

### 在应用中启用 A 记录映射

在 Azure 门户中，返回到应用的“自定义域”页面，将完全限定的自定义 DNS 名称（例如 `contoso.com`）添加到列表。

单击“添加主机名”旁边的 **+** 图标。

![添加主机名](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

键入你为其配置了 A 记录的完全限定的域名，例如 `contoso.com`。

选择“验证”。

“添加主机名”按钮会被激活。 

确保“主机名记录类型”设置为“A 记录 (example.com)”。

选择“添加主机名”。

![将 DNS 名称添加到应用](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

新主机名可能需要经过一段时间后才会反映在应用的“自定义域”页中。 请尝试刷新浏览器来更新数据。

![已添加 A 记录](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

如果之前错过了某个步骤或者在某个位置的输入不正确，则会在页面的底部看到验证错误。

![验证错误](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

<a id="map-a-wildcard-domain" class="xliff"></a>

## 映射通配符域

在教程示例中，你将通过添加 CNAME 记录将[通配符 DNS 名称](https://en.wikipedia.org/wiki/Wildcard_DNS_record)（例如 `*.contoso.com`）映射到应用服务应用。 

<a id="access-dns-records-with-domain-provider" class="xliff"></a>

### 通过域提供商访问 DNS 记录

登录到域提供商的网站。

查找管理 DNS 记录的页面。 每个域提供商都有其自己的 DNS 记录界面，因此应查阅提供商的文档。 查找站点中标签为“域名”、“DNS”或“名称服务器管理”的链接或区域。 

通常，通过查看帐户信息，然后查找如“我的域”之类的链接，便可以找到 DNS 记录管理页面。 转到此页面，然后查找名为**区域文件**、**DNS 记录**或**高级配置**等名称的链接。

以下屏幕截图是 DNS 记录管理页面的一个示例：

![示例 DNS 记录页](./media/app-service-web-tutorial-custom-domain/example-record-ui.png)

在示例屏幕截图中，选择“添加”来创建记录。 某些提供商提供了不同的链接来添加不同的记录类型。 同样，需要查阅提供商的文档。

> [!NOTE]
> 对于某些提供商，在选择单独的“保存更改”链接之前，这些 DNS 记录不会生效。 

<a id="create-the-cname-record" class="xliff"></a>

### 创建 CNAME 记录

添加一条 CNAME 记录来将通配符名称映射到应用的默认主机名 (`<app_name>.chinacloudsites.cn`)。

对于 `*.contoso.com` 域示例，CNAME 记录将名称 `*` 映射到 `<app_name>.chinacloudsites.cn`。

添加 CNAME 后，DNS 记录页面看起来如以下示例：

![在门户中导航到 Azure 应用](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

<a id="enable-the-cname-record-mapping-in-the-app" class="xliff"></a>

### 在应用中启用 CNAME 记录映射

现在，可以向应用中添加与通配符名称匹配的任何子域了（例如，`sub1.contoso.com` 和 `sub2.contoso.com` 与 `*.contoso.com` 匹配）。 

在 Azure 门户中，在应用页面的左侧导航窗格中，选择“自定义域”。 

![自定义域菜单](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

单击“添加主机名”旁边的 **+** 图标。

![添加主机名](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

键入与通配符域匹配的完全限定的域名（例如 `sub1.contoso.com`），然后选择“验证”。

“添加主机名”按钮会被激活。 

确保“主机名记录类型”设置为“CNAME 记录（www.example.com 或任何子域）”。

选择“添加主机名”。

![将 DNS 名称添加到应用](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

新主机名可能需要经过一段时间后才会反映在应用的“自定义域”页中。 请尝试刷新浏览器来更新数据。

再次选择 **+** 图标来添加与通配符域匹配的另一主机名。 例如，添加 `sub2.contoso.com`。

![已添加 CNAME 记录](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

<a id="test-in-browser" class="xliff"></a>

## 在浏览器中测试

浏览到前面配置的 DNS 名称（例如，`contoso.com`、`www.contoso.com`、`sub1.contoso.com` 和 `sub2.contoso.com`）。

![在门户中导航到 Azure 应用](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

<a id="automate-with-scripts" class="xliff"></a>

## 使用脚本自动执行

可以在 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) 或 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) 中使用脚本自动管理自定义域。 

<a id="azure-cli" class="xliff"></a>

### Azure CLI 

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

以下命令将配置的自定义 DNS 名称添加到应用服务应用。 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

有关详细信息，请参阅[将自定义域映射到 Web 应用](scripts/app-service-cli-configure-custom-domain.md)。 

<a id="azure-powershell" class="xliff"></a>

### Azure PowerShell 

以下命令将配置的自定义 DNS 名称添加到应用服务应用。 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.chinacloudsites.cn") 
```

有关详细信息，请参阅[将自定义域分配到 Web 应用](scripts/app-service-powershell-configure-custom-domain.md)。

<a id="next-steps" class="xliff"></a>

## 后续步骤

在本教程中，你已学习了如何执行以下操作：

> [!div class="checklist"]
> * 使用 CNAME 记录映射子域
> * 使用 A 记录映射根域
> * 使用 CNAME 记录映射通配符域
> * 使用脚本自动执行域映射

转到下一教程，了解如何将自定义 SSL 证书绑定到 Web 应用。

> [!div class="nextstepaction"]
> [将现有的自定义 SSL 证书绑定到 Azure Web 应用](app-service-web-tutorial-custom-ssl.md)
