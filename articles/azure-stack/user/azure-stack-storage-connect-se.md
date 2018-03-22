---
title: 将存储资源管理器连接到 Azure Stack 订阅
description: 了解如何将存储资源管理器连接到 Azure Stack 订阅
services: azure-stack
documentationcenter: ''
author: xiaofmao
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
origin.date: 09/25/2017
ms.date: 03/09/2018
ms.author: v-junlch
ms.openlocfilehash: ef9535391e2c73094ca2315c2972bf7bc7995253
ms.sourcegitcommit: af6d48d608d1e6cb01c67a7d267e89c92224f28f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2018
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription"></a>将存储资源管理器连接到 Azure Stack 订阅

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

Azure 存储资源管理器（预览版）是一款独立应用，可用于在 Windows、macOS 和 Linux 上轻松处理 Azure Stack 存储数据。 有几个工具可用于将数据移进/移出 Azure Stack 存储。 有关详细信息，请参阅[适用于 Azure Stack 存储的数据传输工具](azure-stack-storage-transfer.md)。

本文介绍如何使用存储资源管理器连接到 Azure Stack 存储帐户。 

如果尚未安装存储资源管理器，请[下载](http://www.storageexplorer.com/)并安装它。

连接到 Azure Stack 订阅后，可以使用 [Azure 存储资源管理器文章](../../vs-azure-tools-storage-manage-with-storage-explorer.md)处理 Azure Stack 数据。 

## <a name="prepare-an-azure-stack-subscription"></a>准备 Azure Stack 订阅

需要可以访问 Azure Stack 主机桌面或建立 VPN 连接，存储资源管理器才能访问 Azure Stack 订阅。 若要了解如何设置到 Azure Stack 的 VPN 连接，请参阅[使用 VPN 连接到 Azure Stack](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn)。

若是 Azure Stack 开发工具包，需要导出 Azure Stack 颁发机构根证书。

### <a name="to-export-and-then-import-the-azure-stack-certificate"></a>导出 Azure Stack 证书后再导入

1. 在 Azure Stack 主机或已与 Azure Stack 建立 VPN 连接的本地计算机上打开 `mmc.exe`。 

2. 在“文件”中选择“添加/删除管理单元”，并添加“证书”以管理“我的用户帐户”。



3. 在 **Console Root\Certificated (Local Computer)\Trusted Root Certification Authorities\Certificates** 下查找 **AzureStackSelfSignedRootCert**。

    ![通过 mmc.exe 加载 Azure Stack 根证书][25]

4. 右键单击该证书，选择“所有任务” > “导出”，并按说明导出 **Base-64 编码 X.509 (.CER)** 证书。  

    导出的证书会在下一步使用。
5. 启动存储资源管理器（预览版），如果看到“连接到 Azure 存储”对话框，请将其取消。

6. 在“编辑”菜单上，指向“SSL 证书”，并单击“导入证书”。 通过文件选取器对话框找到并打开在上一步导出的证书。

    导入后，系统会提示重新启动存储资源管理器。

    ![将证书导入存储资源管理器（预览版）][27]

现在已准备就绪，可以将存储资源管理器连接到 Azure Stack 订阅。

### <a name="to-connect-an-azure-stack-subscription"></a>连接 Azure Stack 订阅


1. 存储资源管理器（预览版）重新启动以后，即可选择“编辑”菜单，确保选中“目标 Azure Stack”。 如果尚未选中，请将其选中，然后重新启动存储资源管理器，使更改生效。 此配置是必需的，否则无法与 Azure Stack 环境兼容。

    ![确保选中“目标 Azure Stack”][28]

7. 在左窗格中，选择“管理帐户”。  
    此时会显示已登录的所有 Microsoft 帐户。

8. 若要连接到 Azure Stack 帐户，请选择“添加帐户”。

    ![添加 Azure Stack 帐户][29]

9. 在“连接到 Azure 存储”对话框的“Azure 环境”下，选择“使用 Azure Stack 环境”，然后单击“下一步”。

10. 若要使用至少与一个活动 Azure Stack 订阅关联的 Azure Stack 帐户登录，请填写“登录 Azure Stack 环境”对话框。  

    每个字段的详细信息如下所示：

    - **环境名称**：用户可以自定义此字段。
    - **ARM 资源终结点**：Azure 资源管理器资源终结点的示例：

        - 对于云操作员：<br> https://adminmanagement.local.azurestack.external   
        - 对于租户：<br> https://management.local.azurestack.external
 
    - **租户 ID**：可选。 只有在必须指定目录的情况下，才提供此值。

12. 使用 Azure Stack 帐户成功登录后，左窗格将填充与该帐户关联的 Azure Stack 订阅。 选择要使用的 Azure Stack 订阅，并选择“应用”。 （选择或清除“所有订阅”复选框会选择所有列出的 Azure Stack 订阅，或者一个都不选。）

    ![填充“自定义云环境”对话框后，选择 Azure Stack 订阅][30]  
    左窗格会显示与所选 Azure Stack 订阅关联的存储帐户。

    ![存储帐户列表，其中包括 Azure Stack 订阅帐户][31]

## <a name="next-steps"></a>后续步骤
- [存储资源管理器（预览版）入门](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
- [Azure Stack 存储：差异和注意事项](azure-stack-acs-differences.md)


- 若要了解有关 Azure 存储的详细信息，请参阅 [Azure 存储简介](../../storage/common/storage-introduction.md)

[25]: ./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png
[26]: ./media/azure-stack-storage-connect-se/export-root-cert-azure-stack.png
[27]: ./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png
[28]: ./media/azure-stack-storage-connect-se/select-target-azure-stack.png
[29]: ./media/azure-stack-storage-connect-se/add-azure-stack-account.png
[30]: ./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png
[31]: ./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png

