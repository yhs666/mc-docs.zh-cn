---
ms.openlocfilehash: af415ee1796788d38d22f2e890251fb4dbebf35f
ms.sourcegitcommit: b8fb6890caed87831b28c82738d6cecfe50674fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2019
ms.locfileid: "58627775"
---
## <a name="create-a-device-identity"></a>创建设备标识

在本部分中，将使用 [Azure 门户][lnk-azure-portal]在 IoT 中心的标识注册表中创建设备标识。 设备无法连接到 IoT 中心，除非它在标识注册表中具有条目。 有关详细信息，请参阅 [IoT 中心开发人员指南][lnk-devguide-identity]的“标识注册表”部分。 使用门户中的“IoT 设备”面板为设备生成唯一设备 ID 和密钥，以用于在 IoT 中心中标识它本身。 设备 ID 区分大小写。

1. 登录到 [Azure 门户][lnk-azure-portal]。

2. 选择“所有资源”，并查找 IoT 中心资源。

3. 打开 IoT 中心资源后，单击“IoT 设备”工具，并单击顶部的“添加”。 

    ![在门户中创建设备标识][img-add-device]

4. 提供新设备的名称（例如 myDeviceId），然后单击“保存”。 此操作会为 IoT 中心创建新设备标识。

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

   ![添加新设备][img-create-device]

5. 在设备列表中，单击新创建的设备，复制“连接字符串---主键”供以后使用。

    ![设备连接字符串][img-connection-string]
   > [!NOTE]
   > IoT 中心标识注册表仅存储用于实现 IoT 中心安全访问的设备标识。 它存储设备 ID 和密钥作为安全凭据，以及启用/禁用标志让你禁用对单个设备的访问。 如果应用程序需要存储其他特定于设备的元数据，则应使用特定于应用程序的存储。 有关详细信息，请参阅 [IoT 中心开发人员指南][lnk-devguide-identity]。
   > 
   > 

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-add-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png
[img-create-device]:./media/iot-hub-get-started-create-device-identity-portal/add-device.png

<!-- Links -->
[lnk-azure-portal]: https://portal.azure.cn
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

