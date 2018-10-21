---
title: 安装本地数据网关 | Azure
description: 了解如何安装并配置本地数据网关。
author: rockboyfor
manager: digimobile
ms.service: azure-analysis-services
ms.topic: conceptual
origin.date: 09/10/2018
ms.date: 10/22/2018
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: f2b38c30d1bfb8ac13388e97c1a99d96d7a694a4
ms.sourcegitcommit: 2d33477aeb0f2610c23e01eb38272a060142c85d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "49453934"
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>安装并配置本地数据网关
当同一区域中的一个或多个 Azure Analysis Services 服务器连接到本地数据源时，需要本地数据网关。 若要了解有关网关的详细信息，请参阅[本地数据网关](analysis-services-gateway.md)。

## <a name="prerequisites"></a>先决条件
**最低要求：**

* .NET 4.5 Framework
* 64 位版本的 Windows 7/Windows Server 2008 R2（或更高版本）

**推荐：**

* 8 核 CPU
* 8 GB 内存
* 64 位版本的 Windows 2012 R2（或更高版本）

**重要注意事项：**

* 在安装过程中将网关注册到 Azure 时，会选择订阅的默认区域。 可以选择不同的区域。 如果在多个区域中具有服务器，则必须为每个区域安装一个网关。 
* 不能在域控制器上安装网关。
* 一台计算机上只能安装一个网关。
* 在计算机处于开启但未处于休眠状态下安装网关。
* 不要在使用无线网络连接的计算机上安装网关。 否则，可能会降低性能。
* 安装网关时，你用来登录到计算机的用户帐户必须具有“作为服务登录”权限。 安装完成后，本地数据网关服务使用 NT SERVICE\PBIEgwService 帐户作为服务登录。 可以在安装期间指定一个不同的帐户，也可以在安装完成后在“服务”中指定一个不同的帐户。 请确保组策略设置同时允许你在安装时登录的帐户以及你选择的具有“作为服务登录”权限的服务帐户。
* 在 Azure AD 中使用与要在其中注册网关的订阅相同[租户](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)的帐户登录到 Azure。 安装和注册网关时不支持 Azure B2B（来宾）帐户。
* 如果数据源位于 Azure 虚拟网络 (VNet) 上，则必须配置 [AlwaysUseGateway](analysis-services-vnet-gateway.md) 服务器属性。
<!-- * The (unified) gateway described here is not supported in Azure China Cloud, Azure Germany, and Azure China sovereign regions. Use **Dedicated On-premises gateway for Azure Analysis Services**, installed from your server's **Quick Start** in the portal. 

<a name="download"></a>
## Download
 [Download the gateway](https://aka.ms/azureasgateway-mc)

<a name="install"></a>
## Install

1. Run setup.

2. Select a location, accept the terms, and then click **Install**.

   ![Install location and license terms](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. Sign in to Azure. The account must be in your tenant's Azure Active Directory. This account is used for the gateway administrator. Azure B2B (guest) accounts are not supported when installing and registering the gateway.

   ![Sign in to Azure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > If you sign in with a domain account, it's mapped to your organizational account in Azure AD. Your organizational account is used as the gateway administrator.

<a name="register"></a>
## Register
In order to create a gateway resource in Azure, you must register the local instance you installed with the Gateway Cloud Service. 

1.  Select **Register a new gateway on this computer**.

    ![Register](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. Type a name and recovery key for your gateway. By default, the gateway uses your subscription's default region. If you need to select a different region, select **Change Region**.

    > [!IMPORTANT]
    > Save your recovery key in a safe place. The recovery key is required in-order to takeover, migrate, or restore a gateway. 

   ![Register](media/analysis-services-gateway-install/aas-gateway-register-name.png)

<a name="create-resource"></a>
## Create an Azure gateway resource
After you've installed and registered your gateway, you need to create a gateway resource in your Azure subscription. Sign in to Azure with the same account you used when registering the gateway.

1. In Azure portal, click **Create a resource** > **Integration** > **On-premises data gateway**.

   ![Create a gateway resource](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. In **Create connection gateway**, enter these settings:

    * **Name**: Enter a name for your gateway resource. 

    * **Subscription**: Select the Azure subscription 
    to associate with your gateway resource. 

      The default subscription is based on the 
      Azure account that you used to sign in.

    * **Resource group**: Create a resource group or select an existing resource group.

    * **Location**: Select the region you registered your gateway in.

    * **Installation Name**: If your gateway installation isn't already selected, 
    select the gateway registered. 

    When you're done, click **Create**.

<a name="connect-servers"></a>
## Connect servers to the gateway resource

1. In your Azure Analysis Services server overview, click **On-Premises Data Gateway**.

   ![Connect server to gateway](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. In **Pick an On-Premises Data Gateway to connect**, select your gateway resource, and then click **Connect selected gateway**.

   ![Connect server to gateway resource](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > If your gateway does not appear in the list, your server is likely not in the same region as the region you specified when registering the gateway. 

That's it. If you need to open ports or do any troubleshooting, be sure to check out [On-premises data gateway](analysis-services-gateway.md).

## Next steps
* [Manage Analysis Services](analysis-services-manage.md)   
* [Get data from Azure Analysis Services](analysis-services-connect.md)   
* [Use gateway for data sources on an Azure Virtual Network](analysis-services-vnet-gateway.md)

<!-- Update_Description: new articles on analysis services gateway install  -->
<!--ms.date: 10/22/2018-->