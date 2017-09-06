---
title: "使用 Azure 门户配置内容保护策略 | Microsoft Docs"
description: "本文演示如何使用 Azure 门户配置内容保护策略。 本文还说明如何为资产启用动态加密。"
services: media-services
documentationcenter: 
author: hayley244
manager: digimobile
editor: 
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 08/09/2017
ms.date: 09/04/2017
ms.author: v-haiqya
ms.openlocfilehash: 77fde1a47e432fb6076913f9e8192edf5962bc06
ms.sourcegitcommit: 20f589947fbfbe791debd71674f3e4649762b70d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2017
---
# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>使用 Azure 门户配置内容保护策略
> [!NOTE]
> 要完成本教程，需要一个 Azure 帐户。 有关详细信息，请参阅 [Azure 试用版](https://www.azure.cn/pricing/1rmb-trial/)。
> 
> 

## <a name="overview"></a>概述
使用 Azure 媒体服务 (AMS)，可以在媒体从离开计算机到存储、处理和传送的整个过程中确保其安全。 借助媒体服务，可以传送使用高级加密标准（AES，使用 128 位加密密钥）、通用加密（CENC，使用 PlayReady）以及 Apple FairPlay 传送动态加密的内容。 

AMS 提供用于向已授权客户端传送 DRM 许可证和 AES 明文密钥的服务。 借助 Azure 门户，可创建针对所有加密类型的 **密钥/许可证授权策略** 。

本文演示如何使用 Azure 门户配置内容保护策略。 本文还说明如何向资产应用动态加密。


> [!NOTE]
> 如果使用 Azure 经典门户创建了保护策略，这些策略可能不会在 [Azure 门户](https://portal.azure.cn/)中显示。 但是，所有旧策略仍然存在。 可以使用 Azure 媒体服务 .NET SDK 或 [Azure 媒体服务资源管理器](https://github.com/Azure/Azure-Media-Services-Explorer/releases)工具进行检查（若要查看这些策略，右键单击此资产 ->“显示信息 (F4)”->单击“内容密钥”选项卡 ->单击此密钥）。 
> 
> 如果想要使用新策略加密资产，请使用 Azure 门户进行配置，单击“保存”，并重新应用动态加密。 
> 
> 

## <a name="start-configuring-content-protection"></a>开始配置内容保护
如果要使用门户开始配置内容保护，请将 AMS 帐户设置为全局，并执行以下操作：

1. 在 [Azure 门户](https://portal.azure.cn/)中，选择 Azure 媒体服务帐户。
2. 选择“设置” > “内容保护”。

![保护内容](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>密钥/许可证授权策略
AMS 支持通过多种方式对发出密钥或许可证请求的用户进行身份验证。 必须配置内容密钥授权策略且客户端必须遵守该策略，才能将密钥/许可证传送到客户端。 内容密钥授权策略可能受到一种或多种授权限制：开放或令牌限制。

借助 Azure 门户，可创建针对所有加密类型的 **密钥/许可证授权策略** 。

### <a name="open"></a>打开
开放限制意味着系统会将密钥传送给发出密钥请求的任何用户。 此限制可能适用于测试用途。 

### <a name="token"></a>令牌
令牌限制策略必须附带由安全令牌服务 (STS) 颁发的令牌。 媒体服务支持采用简单 Web 令牌 (SWT) 格式和 JSON Web 令牌 (JWT) 格式的令牌。 媒体服务不提供安全令牌服务。 可以创建自定义 STS 或利用 Azure ACS 来颁发令牌。 必须将 STS 配置为创建令牌，该令牌使用指定密钥以及在令牌限制配置中指定的颁发声明进行签名。 如果令牌有效，而且令牌中的声明与为密钥（或许可证）配置的声明相匹配，则媒体服务密钥传送服务会将请求的密钥（或许可证）返回到客户端。

配置令牌限制策略时，必须指定主验证密钥、颁发者和受众参数。 主验证密钥包含用于为令牌签名的密钥，颁发者是颁发令牌的安全令牌服务。 受众（有时称为范围）描述该令牌的意图，或者令牌授权访问的资源。 媒体服务密钥传送服务验证令牌中的这些值是否与模板中的值匹配。

![保护内容](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>PlayReady 权限模板
有关 PlayReady 权限模板的详细信息，请参阅[媒体服务 PlayReady 许可证模板概述](media-services-playready-license-template-overview.md)。

### <a name="non-persistent"></a>非持久
如果将许可证配置为非持久，则仅当播放器使用该许可证时才会在内存中保存该许可证。  

![保护内容](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>持久
如果将许可证配置为持久，则会在客户端的持久性存储中保存该许可证。

![保护内容](./media/media-services-portal-content-protection/media-services-content-protection004.png)


## <a name="fairplay-configuration"></a>FairPlay 配置
若要启用 FairPlay 加密，需要通过 FairPlay 配置选项提供应用证书和应用程序密钥 (ASK)。 有关 FairPlay 配置和要求的详细信息，请参阅[此](media-services-protect-hls-with-fairplay.md)文。

![保护内容](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>向资产应用动态加密
要利用动态加密，需要将源文件编码为一组自适应比特率 MP4 文件。

### <a name="select-an-asset-that-you-want-to-encrypt"></a>选择想要加密的资产
若要查看所有资产，选择“设置” > “资产”。

![保护内容](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>使用 AES 或 DRM 加密
在资产上按“加密”后，有两种选择：**AES** 或 **DRM**。 

#### <a name="aes"></a>AES
所有流式处理协议上都会启用 AES 明文密钥加密：平滑流式处理、HLS 和 MPEG-DASH。

![保护内容](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
如果选择“DRM”选项卡，可选择不同的内容保护策略（此时必须已配置）以及一组流式处理协议。

* **仅 PlayReady 与平滑流式处理、HLS 和 MPEG-DASH** 会使用 PlayReady DRM 动态加密平滑流式处理、HLS、MPEG-DASH 流。
* **仅 FairPlay 与 HLS** 会使用 FairPlay 动态加密 HLS 流。

若要启用 FairPlay 加密，则需要通过“内容保护设置”边栏选项卡的“FairPlay 配置”选项提供应用证书和应用程序密钥 (ASK)。

![保护内容](./media/media-services-portal-content-protection/media-services-content-protection009.png)

完成加密选择后，按“应用” 。

