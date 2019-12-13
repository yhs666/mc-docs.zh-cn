---
title: 管理 Azure IoT Central 应用程序 | Microsoft Docs
description: 本文介绍管理员如何通过更改应用程序名称和 URL 来管理 Azure IoT Central 应用程序，以及如何上传映像、复制和删除应用程序
author: viv-liu
ms.author: v-yiso
origin.date: 08/26/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: df907fd3b26581a45983a73b75da348ba6807d72
ms.sourcegitcommit: 6ffa4d50cee80c7c0944e215ca917a248f2a4bcd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74883742"
---
# <a name="manage-your-iot-central-application"></a>管理 IoT Central 应用程序

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

本文为管理员介绍如何通过更改应用程序名称和 URL 以及上传图像来管理应用程序，此外介绍如何在 Azure IoT Central 应用程序中复制和删除应用程序。

只有 Azure IoT Central 应用程序的“管理员”  角色才能访问和使用“管理”  部分。 如果你创建了 Azure IoT Central 应用程序，则会自动分配到该应用程序的“管理员”  角色。 

## <a name="change-application-name-and-url"></a>更改应用程序名称和 URL

在“应用程序设置”页中，可以更改应用程序的名称和 URL，然后选择“保存”。  

![“应用程序设置”页](media/howto-administer/image0-a.png)

如果管理员为应用程序创建了自定义主题，此页将包含一个用于在 UI 中隐藏“应用程序名称”的选项。  如果自定义主题中的应用程序徽标包含应用程序名称，则此选项非常有用。 有关详细信息，请参阅[自定义 Azure IoT Central UI](./howto-customize-ui.md)。

> [!Note]
> 如果更改了 URL，旧 URL 可由其他 Azure IoT Central 客户使用。 如果出现此情况，你再也不能使用旧 URL。 更改 URL 后，旧 URL 不再有效，因此需要告知用户要使用的新 URL。

## <a name="prepare-and-upload-image"></a>准备和上传图像

若要更改应用程序图像，请参阅[准备图像并将其上传到 Azure IoT Central 应用程序](howto-prepare-images.md)。

## <a name="copy-an-application"></a>复制应用程序

可以创建任一应用程序的副本，但其中不会包括任何设备实例、设备数据历史记录和用户数据。 该副本是付费的即用即付应用程序。 不能以这种方式创建试用版应用程序。

选择“复制”。  在对话框中，输入新的即用即付应用程序的详细信息。 再次选择“复制”以确认要继续操作。  在[创建应用程序](quick-deploy-iot-central.md)快速入门中详细了解此窗体中的字段。

![“应用程序设置”页](media/howto-administer/appcopy2.png)

应用复制操作成功后，可以使用链接导航到新应用程序。

![“应用程序设置”页](media/howto-administer/appcopy3a.png)

复制某个应用程序也会复制规则和电子邮件操作的定义。 某些操作（例如 Flow 和逻辑应用等）通过规则 ID 与特定的规则相关联。 将某个规则复制到另一应用程序后，该规则将获得自身的规则 ID。 在这种情况下，用户必须创建一个新的操作，然后将新规则与该操作相关联。 一般而言，最好是检查规则和操作，以确保它们在新应用中是最新的。

> [!WARNING]
> 如果仪表板包含显示特定设备相关信息的磁贴，则这些磁贴会在新应用程序中显示“找不到请求的资源”。  必须重新配置这些磁贴才能显示有关新应用程序中设备的信息。

## <a name="delete-an-application"></a>删除应用程序

使用“删除”按钮可以永久删除 IoT Central 应用程序。  此操作会永久删除与该应用程序关联的所有数据。

> [!Note]
> 若要删除某个应用程序，还必须有权删除在创建应用程序时所选的 Azure 订阅中的资源。 有关详细信息，请参阅[使用基于角色的访问控制来管理对 Azure 订阅资源的访问权限](/active-directory/role-based-access-control-configure)。


## <a name="manage-programatically"></a>以编程方式进行管理

IoT Central Azure 资源管理器 SDK 程序包适用于 Node、Python、C#、Ruby、Java 和 Go。 可以使用这些包来创建、列出、更新或删除 IoT Central 应用程序。 这些包包含用于管理身份验证和错误处理的帮助程序。

可以在 [https://github.com/emgarten/iotcentral-arm-sdk-examples](https://github.com/emgarten/iotcentral-arm-sdk-examples) 中找到有关如何使用 Azure 资源管理器 SDK 的示例。

有关详细信息，请参阅以下 GitHub 存储库和包：

| 语言 | 存储库 | 程序包 |
| ---------| ---------- | ------- |
| 节点 | [https://github.com/Azure/azure-sdk-for-node](https://github.com/Azure/azure-sdk-for-node) | [https://www.npmjs.com/package/azure-arm-iotcentral](https://www.npmjs.com/package/azure-arm-iotcentral)
| Python |[https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) | [https://pypi.org/project/azure-mgmt-iotcentral](https://pypi.org/project/azure-mgmt-iotcentral)
| C# | [https://github.com/Azure/azure-sdk-for-net](https://github.com/Azure/azure-sdk-for-net) | [https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral](https://www.nuget.org/packages/Microsoft.Azure.Management.IotCentral)
| Ruby | [https://github.com/Azure/azure-sdk-for-ruby](https://github.com/Azure/azure-sdk-for-ruby) | [https://rubygems.org/gems/azure_mgmt_iot_central](https://rubygems.org/gems/azure_mgmt_iot_central)
| Java | [https://github.com/Azure/azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) | [https://search.maven.org/search?q=a:azure-mgmt-iotcentral](https://search.maven.org/search?q=a:azure-mgmt-iotcentral)
| Go | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go) | [https://github.com/Azure/azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)

## <a name="next-steps"></a>后续步骤
 
了解如何管理 Azure IoT Central 应用程序后，建议接下来了解如何在 Azure IoT Central 中[管理用户和角色](howto-manage-users-roles.md)。