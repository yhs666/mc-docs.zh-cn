---
title: 从 Azure 逻辑应用中访问本地数据源 | Microsoft Docs
description: 通过创建本地数据网关，从逻辑应用连接到本地数据源
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: v-yiso
ms.reviewer: arthii, LADocs
ms.topic: article
origin.date: 09/01/2019
ms.date: 10/08/2019
ms.openlocfilehash: 06346ec65e1d8561d647fbeb56883ef259b2e3da
ms.sourcegitcommit: 332ae4986f49c2e63bd781685dd3e0d49c696456
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340919"
---
# <a name="connect-to-on-premises-data-sources-from-azure-logic-apps"></a>从 Azure 逻辑应用连接到本地数据源

若要从逻辑应用中访问本地数据源，请在 Azure 门户中创建本地数据网关资源。 然后，逻辑应用可以使用[本地连接器](../connectors/apis-list.md#on-premises-connectors)。 本文介绍[在本地计算机上下载并安装网关](../logic-apps/logic-apps-gateway-install.md)之后，如何创建 Azure 网关资源。  有关网关的详细信息，请参阅[网关的工作原理](../logic-apps/logic-apps-gateway-install.md#gateway-cloud-service)。


有关如何将网关用于其他服务的信息，请参阅以下文章：

* [Microsoft Power BI 本地数据网关](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Microsoft Flow 本地数据网关](https://flow.microsoft.com/documentation/gateway-manage/)
* [Microsoft PowerApps 本地数据网关](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Azure Analysis Services 本地数据网关](../analysis-services/analysis-services-gateway.md)

<a name="supported-connections"></a>

## <a name="supported-data-sources"></a>支持的数据源

Azure 逻辑应用的本地数据网关支持以下数据源的[本地连接器](../connectors/apis-list.md#on-premises-connectors)：

* BizTalk Server 2016
* 文件系统
* IBM DB2  
* IBM Informix
* IBM MQ
* MySQL
* Oracle 数据库
* PostgreSQL
* SAP
* SharePoint Server
* SQL Server
* Teradata

虽然网关本身不额外收费，但[逻辑应用定价模型](../logic-apps/logic-apps-pricing.md)适用于这些连接器以及 Azure 逻辑应用中的其他操作。

## <a name="prerequisites"></a>先决条件

* 已经[在本地计算机上安装本地数据网关](../logic-apps/logic-apps-gateway-install.md)。

* 你的 [Azure 帐户和 Azure 订阅](../logic-apps/logic-apps-gateway-install.md#requirements)与安装本地数据网关时使用的帐户和订阅相同。

* 尚未将网关安装关联到 Azure 中的另一个网关资源。

  创建网关资源时，你选择一个与网关资源相关联的网关安装。 创建网关资源时，不能选择某个已关联的网关安装。

<a name="create-gateway-resource"></a>

## <a name="create-azure-gateway-resource"></a>创建 Azure 网关资源

在本地计算机上安装网关后，请为网关创建 Azure 资源。 

1. 使用安装网关时所使用的 Azure 帐户登录到 [Azure 门户](https://portal.azure.cn)。

1. 在 Azure 门户搜索框中输入“本地数据网关”，然后选择“本地数据网关”  。

   ![查找“本地数据网关”](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

1. 在“本地数据网关”下，选择“添加”。  

   ![添加数据网关](./media/logic-apps-gateway-connection/add-gateway.png)

1. 在“创建连接网关”下，提供网关资源的以下信息。  完成操作后，选择“创建”  。

   | 属性 | 说明 | 
   |----------|-------------|
   | 资源名称  | 网关资源名称，只能包含字母、数字、连字符 (`-`)、下划线 (`_`)、括号（`(`、`)`）和句点 (`.`)。 |
   | **订阅** | 你的 Azure 订阅，必须与网关安装和逻辑应用的相同。 默认订阅取决于用来登录的 Azure 帐户。 |
   | **资源组** | 要使用的 [Azure 资源组](../azure-resource-manager/resource-group-overview.md) |
   | **Location** | 此位置所在的区域必须是在[网关安装](../logic-apps/logic-apps-gateway-install.md)期间为网关云服务选择的区域。 否则，网关安装不会显示在“安装名称”列表中，因此无法选择。  逻辑应用位置可能不同于网关资源位置。 |
   | **安装名称** | 如果尚未选择网关安装，请选择前面安装的网关。 以前关联的网关安装不会显示在此列表中，因此无法选择。 |
   |||

   以下是示例：

   ![提供创建本地数据网关所需的详细信息](./media/logic-apps-gateway-connection/gateway-details.png)

<a name="connect-logic-app-gateway"></a>

## <a name="connect-to-on-premises-data"></a>连接到本地数据

创建网关资源并将 Azure 订阅与此资源相关联后，可以使用该网关在逻辑应用与本地数据源之间创建连接。

1. 在 Azure 门户的逻辑应用设计器中创建或打开逻辑应用。

1. 添加支持本地连接的连接器，例如 **SQL Server**。

1. 选择“通过本地数据网关连接”。  

1. 对于“网关”，请选择已创建的网关资源。 

   > [!NOTE]
   > 网关列表包含其他区域的网关资源，因为逻辑应用的位置可能不同于网关资源的位置。

1. 提供唯一的连接名称和其他所需信息，具体取决于要创建的连接。

   唯一的连接名称有助于在以后轻松查找该连接，尤其是在创建多个连接的情况下。 另请包括用户名的限定域（如果适用）。
   
      以下是示例：

   ![在逻辑应用和数据网关之间创建连接](./media/logic-apps-gateway-connection/logic-app-gateway-connection.png)

1. 完成操作后，选择“创建”  。 

网关连接现在已准备就绪，可供逻辑应用使用。

## <a name="edit-connection"></a>编辑连接

若要更新网关连接的设置，可以编辑连接。

1. 若要查找逻辑应用的所有 API 连接，请在逻辑应用菜单中的“开发工具”下，选择“API 连接”。  
   
   ![在逻辑应用菜单上，选择“API 连接”](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

1. 选择所需的网关连接，然后选择“编辑 API 连接”。 

   > [!TIP]
   > 如果更新未生效，请尝试针对网关安装[停止网关 Windows 服务帐户，然后重启该帐户](../logic-apps/logic-apps-gateway-install.md#restart-gateway)。

查找与 Azure 订阅关联的所有 API 连接： 

* 在 Azure 主菜单中，转到“所有服务” > “Web” > “API 连接”。   
* 或者，在 Azure 主菜单中转到“所有资源”  。 将“类型”  筛选器设置为“API 连接”。 

<a name="change-delete-gateway-resource"></a>

## <a name="delete-gateway-resource"></a>删除网关资源

若要创建其他网关资源、将网关安装与其他网关资源相关联，或者移除网关资源，则可删除网关资源，不影响网关安装。 

1. 在 Azure 主菜单中，选择“所有资源”  。 找到并选择所需的网关资源。

1. 如果尚未选择，请在网关资源菜单中选择“本地数据网关”。  在网关资源工具栏上，选择“删除”。 

   例如：

   ![删除网关](./media/logic-apps-gateway-connection/gateway-delete.png)

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>常见问题

**问**：在 Azure 中创建网关资源时为何看不到我的网关安装？ <br/>
**答**：此问题的可能原因如下：

* 网关安装已由 Azure 中的另一个网关资源注册并声明。 为网关安装创建网关资源后，实例列表中不会显示这些网关安装。 若要在 Azure 门户中检查网关注册，请查看所有 Azure 订阅的“本地数据网关”类型的所有 Azure 资源。  

* 网关安装人员的 Azure AD 标识不同于登录 Azure 门户的人员。 请检查你是否使用了用于安装网关的同一标识登录。

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>后续步骤

* [保护逻辑应用](./logic-apps-securing-a-logic-app.md)
* [逻辑应用的常见示例和方案](./logic-apps-examples-and-scenarios.md)
