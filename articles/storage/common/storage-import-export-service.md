---
title: 使用 Azure 导入/导出将数据传入/传出 Azure 存储 | Azure
description: 了解如何在 Azure 门户中创建导入和导出作业，以便将数据传入/传出到 Azure 存储。
author: yunan2016
manager: digimobile
services: storage
ms.service: storage
ms.topic: article
origin.date: 07/11/2018
ms.date: 07/30/2018
ms.author: v-nany
ms.openlocfilehash: 32164067929764f0760bde2bac1c9b95b23d46b1
ms.sourcegitcommit: 547436d67011c6fe58538cfb60b5b9c69db1533a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52676924"
---
# <a name="use-the-azure-importexport-service-to-transfer-data-to-azure-storage"></a>使用 Azure 导入/导出服务将数据传输到 Azure 存储
本文分步介绍如何使用 Azure 导入/导出服务将磁盘驱动器寄送到 Azure 数据中心，从而安全地将大量数据传输到 Azure Blob 存储和 Azure 文件。 此外，还可以使用此服务将数据从 Azure 存储传输到硬盘驱动器，然后再寄送到本地站点。 可将单个内部 SATA 磁盘驱动器中的数据导入 Azure Blob 存储或 Azure 文件。 

> [!IMPORTANT] 
> 此服务仅接受内部 SATA HDD 或 SSD。 不支持任何其他设备。 请勿发送外部 HDD、NAS 设备等，因为可能会将其返回（若可能）或丢弃。
>
>

如果将磁盘上的数据导入 Azure 存储，请执行以下步骤。
### <a name="step-1-prepare-the-drives-using-waimportexport-tool-and-generate-journal-files"></a>步骤 1：使用 WAImportExport 工具准备驱动器，并生成日志文件。

1.  标识要导入 Azure 存储的数据。 可以导入本地服务器或网络共享中的目录和独立文件。
2.  根据数据的总大小，采购所需数目的 2.5 英寸 SSD 或者 2.5/3.5 英寸 SATA II 或 III 硬盘驱动器。
3.  直接使用 SATA 或通过外部 USB 适配器将硬盘驱动器附加到 Windows 计算机。
1.  在每个硬盘驱动器上创建一个 NTFS 卷，并向该卷分配一个驱动器号。 没有装入点。
2.  若要在 Windows 计算机上启用加密，请启用 NTFS 卷上的 bit locker 加密。 请使用 https://technet.microsoft.com/library/cc731549(v=ws.10).aspx 中的说明。
3.  使用复制粘贴或拖放操作，或使用 RoboCopy 或任何类似的工具将数据完全复制到磁盘上这些加密的 NTFS 卷。
7.  从 https://www.microsoft.com/en-us/download/details.aspx?id=42659 下载 WAImportExport V1
8.  解压缩到默认文件夹 waimportexportv1。 例如，C:\WaImportExportV1  
9.  以管理员身份运行并打开 PowerShell 或命令行，并将目录更改为解压缩的文件夹。 例如，cd C:\WaImportExportV1
10. 将以下命令行复制到文本编辑器并进行编辑，以创建命令行：

    ```
    ./WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:***== /t:D /bk:*** /srcdir:D:\ /dstdir:ContainerName/ /skipwrite 
    ```
    
    下表介绍了这些命令行选项：

    |选项  |说明  |
    |---------|---------|
    |/j:     |带有 .jrn 扩展名的日志文件的名称。 会为每个驱动器生成一个日志文件。 建议使用磁盘序列号作为日志文件名。         |
    |/sk:     |Azure 存储帐户密钥。         |
    |/t:     |要寄送的磁盘的驱动器号。 例如，驱动器 `D`。         |
    |/bk:     |驱动器的 BitLocker 密钥。 其数字密码来自 ` manage-bde -protectors -get D: ` 的输出      |
    |/srcdir:     |要寄送的磁盘的驱动器号后跟 `:\`。 例如，`D:\`。         |
    |/dstdir:     |Azure 存储中的目标容器的名称         |
    |/skipwrite:     |此选项指定没有需要复制的新数据以及要准备磁盘上的现有数据         |
1. 为每个需要寄送的磁盘重复步骤 10。
2. 每次运行命令行时，都会创建使用 /j： 参数提供名称的日志文件。

### <a name="step-2-create-an-import-job-on-azure-portal"></a>步骤 2：在 Azure 门户中创建导入作业。

1. 登录 https://portal.azure.com/，在“更多服务”->“存储”->“导入/导出作业”下，单击“创建导入/导出作业”。

2. 在“基本”部分中，选择“导入 Azure”，输入作业名称字符串，选择订阅，输入或选择资源组。 输入导入作业的描述性名称。 请注意，输入的名称只能包含小写字母、数字、连字符和下划线，必须以字母开头并且不得包含空格。 在作业进行中以及作业完成后，使用所选名称来跟踪作业。

3. 在“作业详细信息”部分中，上传在驱动器准备步骤中获取的驱动器日志文件。 如果使用了 waimportexport.exe version1，需要为已准备好的每个驱动器上传一个文件。 在“导入目标”存储帐户部分中，选择要将数据导入哪个存储帐户。 “放置位置”根据选定存储帐户所属的区域自动进行填充。
   
   ![创建导入作业 - 步骤 3](./media/storage-import-export-service/import-job-03.png)
4. 在“回寄信息”部分中，从下拉列表中选择快递商，并输入已通过此快递商创建的有效快递商帐号。 当导入作业完成后，我们使用此帐户寄回驱动器。 输入完整、有效的联系人姓名、电话号码、电子邮件地址、街道地址、城市、邮政编码、州/省/自治区/直辖市和国家/地区。
   
5. 在“摘要”部分中，输入 Azure 数据中心寄送地址是为了将磁盘寄送到 Azure 数据中心。 请确保寄送标签上标明了作业名称和完整地址。 

6. 单击“摘要页”上的“确定”，完成“创建导入作业”。

### <a name="step-3-ship-the-drives-to-the-azure-datacenter-shipping-address-provided-in-step-2"></a>步骤 3：将驱动器寄送到步骤 2 中提供的 Azure 数据中心寄送地址。
可以使用 FedEx 或中国邮政将包裹寄送到 Azure DC。

### <a name="step-4-update-the-job-created-in-step2-with-tracking-number-of-the-shipment"></a>步骤 4：使用货物的跟踪号更新在步骤 2 中创建的作业。
寄送磁盘后，返回到 Azure 门户上的“导入/导出”页，通过以下步骤更新跟踪号：a) 转到并单击导入作业；b) 单击“驱动器寄送后，更新作业状态和跟踪信息”。 c) 选中“标记为‘已寄送’”复选框；d) 输入“快递商”和“跟踪号码”。
如果在创建作业后的 2 周内未更新跟踪号，该作业便会过期。 可以在门户仪表板上跟踪作业进度。 若要了解上一部分中每个作业状态的含义，请 [查看作业状态](#viewing-your-job-status)。

## <a name="when-should-i-use-the-azure-importexport-service"></a>应该在什么时候使用 Azure 导入/导出服务？
如果通过网络上传或下载数据速度太慢，或者获取额外的网络带宽因成本过高而受到限制，则可考虑使用 Azure 导入/导出服务。

下面这样的场景可以使用此服务：

* 将数据迁移到云：将大量数据快速且经济高效地转移到 Azure。
* 内容分发：将数据快速发送到客户站点。
* 备份：将本地数据备份后存储在 Azure 存储中。
* 数据恢复：恢复存储在存储中的大量数据，然后将其递送到本地位置。

## <a name="prerequisites"></a>先决条件
本部分列出了使用此服务需要满足的先决条件。 在寄送驱动器之前仔细查看这些先决条件。

### <a name="storage-account"></a>存储帐户
必须拥有 Azure 订阅以及一个或多个存储帐户才能使用导入/导出服务。 Azure 导入/导出仅支持经典、Blob 存储帐户和常规用途 v1 存储帐户。 每个作业只能用于将数据传入/传出一个存储帐户。 换言之，一个导入/导出作业不能跨多个存储帐户。 有关创建新存储帐户的信息，请参阅[如何创建存储帐户](storage-quickstart-create-account.md)。

> [!IMPORTANT] 
> Azure 导入导出服务不支持已启用 [虚拟网络服务终结点](../../virtual-network/virtual-network-service-endpoints-overview.md) 功能的存储帐户。 
> 
> 

### <a name="data-types"></a>数据类型
可使用 Azure 导入/导出服务将数据复制到“块”Blob、“页”Blob 或文件中。 与之相反，只能使用此服务从 Azure 存储中导出块 blob、页 blob 或追加 blob。 该服务仅支持将 Azure 文件导入到 Azure 存储。 当前暂不支持导出 Azure 文件。

### <a name="job"></a>作业
要开始从存储进行导入或导出的过程，首先要创建一个“作业”。 作业可以是“导入作业”或“导出作业”：

* 需要将本地数据传输到 Azure 存储帐户时，可创建导入作业。
* 若要将当前在存储帐户中存储的数据传输到寄送给我们的硬盘，请创建导出作业。 创建作业时，需通知导入/导出服务：要将一个或多个硬盘驱动器运送到 Azure 数据中心。

* 对于导入作业，需要寄送包含数据的硬盘驱动器。
* 对于导出作业，需要寄送空硬盘驱动器。
* 每个作业最多可以寄送 10 个硬盘驱动器。

可以使用 Azure 门户或 [Azure 存储导入/导出 REST API](https://docs.microsoft.com/rest/api/storageimportexport) 创建导入或导出作业。

> [!Note]
> 自 2018 年 2 月 28 日起将不再支持 RDFE API。 若要继续使用此服务，请迁移到 [ARM 导入/导出 REST API](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/storageimportexport/resource-manager/Microsoft.ImportExport/stable/2016-11-01/storageimportexport.json)。 

### <a name="waimportexport-tool"></a>WAImportExport 工具
创建 **导入** 作业的第一步是准备需要通过寄送进行数据导入的驱动器。 要准备驱动器，必须将其连接到本地服务器，然后在本地服务器上运行 WAImportExport 工具。 使用此 WAImportExport 工具可以方便地将数据复制到驱动器、使用 BitLocker 加密驱动器上的数据，以及生成驱动器日志文件。

日志文件存储有关作业和驱动器的基本信息，例如驱动器序列号和存储帐户名称。 此日志文件不存储在该驱动器上。 它在导入作业创建期间使用。 本文后面提供了有关作业创建过程的分步详细信息。

WAImportExport 工具仅兼容 64 位 Windows 操作系统。 请参阅 [操作系统](#operating-system) 部分以了解受支持的特定 OS 版本。

下载最新版本的 [WAImportExport 工具](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExportV2.zip)。 有关使用 WAImportExport 工具的详细信息，请参阅[使用 WAImportExport 工具](storage-import-export-tool-how-to.md)。

>[!NOTE]
>以前的版本：可以[下载 WAImportExpot V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip) 版本的工具，并参考 [WAImportExpot V1 使用指南](storage-import-export-tool-how-to-v1.md)。 WAImportExpot V1 版本的工具支持在已将数据预先写入磁盘的情况下准备磁盘。 如果唯一可用的密钥是 SAS 密钥，则也需要 WAImportExpot V1 工具。

>

### <a name="hard-disk-drives"></a>硬盘驱动器
只支持将 2.5 英寸 SSD 或者 2.5/3.5 英寸 SATA II 或 III 内部 HDD 用于导入/导出服务。 单个导入/导出作业最多可以有 10 个 HDD/SSD，并且每个 HDD/SSD 可以为任意大小。 大量驱动器可在多个作业中分布，且对可创建的作业数量没有限制。 

对于导入作业，仅处理驱动器上的第一个数据卷。 该数据卷必须使用 NTFS 进行格式化。

> [!IMPORTANT]
> 此服务不支持内置 USB 适配器附带的外部硬盘驱动器。 此外，不能使用外部 HDD 包装内的磁盘；请勿寄送外部 HDD。
> 
> 

以下是用于将数据复制到内部 HDD 的外部 USB 适配器列表。 Anker 68UPSATAA-02BU Anker 68UPSHHDS-BU Startech SATADOCK22UE Orico 6628SUS3-C-BK（6628 系列）Thermaltake BlacX Hot-Swap SATA External Hard Drive Docking Station（USB 2.0 和 eSATA）

### <a name="encryption"></a>Encryption
必须使用 BitLocker 驱动器加密对驱动器上的数据进行加密。 此加密会在运送过程中保护数据。

对于导入作业，可以通过两种方式进行加密。 第一种方式是在运行 WAImportExport 工具准备驱动器时，使用数据集 CSV 文件指定该选项。 第二种方式是在准备驱动器期间，在驱动器上手动启用 BitLocker 加密并在运行 WAImportExport 工具命令行时，在驱动集 CSV 文件中指定加密密钥。

对于导出作业，在将你的数据复制到驱动器以后，此服务会使用 BitLocker 加密驱动器，此后再将驱动器寄回给你。 加密密钥是通过 Azure 门户提供的。  

### <a name="operating-system"></a>操作系统
在将驱动器寄送到 Azure 之前，可以使用下述 64 位操作系统之一通过 WAImportExport 工具准备硬盘驱动器：

Windows 7 Enterprise、Windows 7 Ultimate、Windows 8 Pro、Windows 8 Enterprise、Windows 8.1 Pro、Windows 8.1 Enterprise、Windows 10、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2。 所有这些操作系统都支持 BitLocker 驱动器加密。

### <a name="locations"></a>位置
Azure 导入/导出服务支持将数据复制到所有公共 Azure 存储帐户，以及从后者进行复制。 可以将硬盘驱动器寄送到列出的其中一个位置。 如果存储帐户所在的公共 Azure 位置没有在这里指定，则使用 Azure 门户或导入/导出 REST API 创建作业时，会提供备用的寄送位置。

支持的寄送位置：
- 中国东部
- 中国北部

### <a name="shipping"></a>装运
**将驱动器寄送到数据中心：**

创建导入或导出作业时，会向你提供某个受支持位置的寄送地址，以便你寄送自己的驱动器。 提供的寄送地址取决于存储帐户的位置，但可能不同于存储帐户位置。

可以通过 FedEx 或中国邮政等快递商将驱动器寄送到寄送地址。

**从数据中心寄送驱动器：**

创建导入或导出作业时，必须提供回寄地址，以便 21Vainet 在作业完成后使用该地址寄回驱动器。 请确保提供有效的回寄地址，以免延误对驱动器的处理。

该承运人商应提供相应的跟踪号来维护监护链。 此外，还必须提供有效的中国邮政服务帐号，以便我们寄回驱动器。 如果已经有了一个快递商帐户号码，请确保其有效。

寄送包裹时，必须遵循 [Azure 服务条款](https://www.azure.cn/support/legal/services-terms/)中的条款。

> [!IMPORTANT]
> 请注意，发运的物理介质可能需要穿越国界。 应当负责确保物理介质和数据是遵照适用的法律导入和/或导出的。 在寄送物理介质之前，请咨询顾问以验证介质和数据是否可以合法地寄送到所确定的数据中心。 这有助于确保它可以及时到达 Microsoft。 例如，任何跨国界的包裹都需要附上商业发票（除非在欧盟内跨越国界）。 可从快递商网站打印填写好的商业发票的副本。 比如，商业发票可以是 [FedEx 商业发票](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf)。 请确保 Microsoft 未被指定为导出者。
> 
> 

## <a name="how-does-the-azure-importexport-service-work"></a>Azure 导入/导出服务是如何工作的？
可以使用 Azure 导入/导出服务在本地站点和 Azure 存储之间传输数据，只需创建作业，然后将硬盘驱动器寄送到 Azure 数据中心即可。 寄送的每个硬盘驱动器都与单个作业相关联。 每个作业都与单个存储帐户相关联。 请仔细查看[先决条件](#pre-requisites)部分，以了解此服务的具体情况，例如支持的数据类型、磁盘类型、位置和寄送方式。

此部分介绍作业导入和导出操作中涉及的高级别步骤。 随后，将在[快速启动部分](#quick-start)提供分步说明，指导如何创建导入和导出作业。

### <a name="inside-an-import-job"></a>关于导入作业
概括而言，导入作业包括以下步骤：

* 确定要导入的数据，以及所需驱动器数目。
* 确定 Azure 存储中用于存储数据的目标 Blob 或文件位置。
* 使用 WAImportExport 工具将数据复制到一个或多个硬盘驱动器，并使用 BitLocker 进行加密。
* 使用 Azure 门户或导入/导出 REST API 在目标存储帐户中创建导入作业。 如果使用 Azure 门户，请上传驱动器日记文件。
* 请提供回寄地址以及快递商帐户号码，以便我们将驱动器寄回给你。
* 将硬盘驱动器寄送到在创建作业时获得的寄送地址。
* 在导入作业详细信息中更新快递跟踪号码，并提交导入作业。
* Azure 数据中心在收到驱动器后会对其进行处理。
* 该中心会使用快递商帐户将驱动器寄送到在导入作业中提供的回寄地址。
  
    ![图 1：导入作业流](./media/storage-import-export-service/importjob.png)

### <a name="inside-an-export-job"></a>关于导出作业
> [!IMPORTANT]
> 该服务仅支持导出 Azure Blob，不支持导出 Azure 文件。
> 
>

概括而言，导出作业包括以下步骤：

* 确定要导出的数据，以及所需驱动器数目。
* 确定数据在 Blob 存储中的源 Blob 或容器路径。
* 使用 Azure 门户或导入/导出 REST API 在源存储帐户中创建导出作业。
* 指定数据在导出作业中的源 Blob 或容器路径。
* 请提供回寄地址以及快递商帐户号码，以便我们将驱动器寄回给你。
* 将硬盘驱动器寄送到在创建作业时获得的寄送地址。
* 在导出作业详细信息中更新快递跟踪号码，并提交导出作业。
* Azure 数据中心在收到驱动器后会对其进行处理。
* 驱动器使用 BitLocker 加密；密钥通过 Azure 门户提供。  
* 该中心会使用快递商帐户将驱动器寄送到在导入作业中提供的回寄地址。
  
    ![图 2：导出作业流](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-and-drive-status"></a>查看作业和驱动器状态
可以从 Azure 门户跟踪导入或导出作业的状态。 单击“导入/导出”选项卡。随即页面上显示作业列表。

![查看作业状态](./media/storage-import-export-service/jobstate.png)

会看到以下作业状态之一，具体取决于驱动器处于哪个处理阶段。

| 作业状态 | 说明 |
|:--- |:--- |
| Creating | 创建一个作业后，其状态设置为 Creating。 当作业处于 Creating 状态时，导入/导出服务假设驱动器尚未寄送到数据中心。 作业可保持 Creating 状态最多两周，之后服务会自动将其删除。 |
| 装运 | 寄出包裹后，应在 Azure 门户中更新跟踪信息。  这会将作业转为“Shipping”状态。 作业会保持 Shipping 状态最多两周。 
| Received | 数据中心中收到所有驱动器后，作业状态会设置为 Received。 |
| 转移 | 至少已开始处理一个驱动器后，作业状态会设置为 Transferring。 有关详细信息，请参阅下面的“驱动器状态”部分。 |
| 打包 | 处理完所有驱动器后，作业会进入 Packaging 状态，直到将驱动器寄回给你。 |
| 已完成 | 将所有驱动器寄回给客户后，如果完成作业时未出现错误，则会将该作业设置为 Completed 状态。 作业在保持 Completed 状态 90 天后自动删除。 |
| 已关闭 | 将所有驱动器寄回给客户后，如果在处理作业期间出现过错误，则会将该作业设置为 Closed 状态。 作业在保持 Closed 状态 90 天后自动删除。 |

下表描述了单个驱动器在导入或导出作业中转换时的生命周期。 现在，作业中每个驱动器的当前状态都会在 Azure 门户中显示。
下表描述了每个驱动器在作业中可能经历的每种状态。

| 驱动器状态 | 说明 |
|:--- |:--- |
| Specified | 对于导入作业，在 Azure 门户中创建作业时，驱动器的初始状态为 Specified。 对于导出作业，由于在创建该作业时未指定驱动器，因此驱动器的初始状态为 Received。 |
| Received | 导入/导出服务操作员为导入作业处理从货运公司收到的驱动器后，驱动器转换为 Received 状态。 对于导出作业，初始驱动器状态为 Received。 |
| NeverReceived | 当作业的包裹已送达但包裹不包含驱动器时，驱动器将转换为 NeverReceived 状态。 如果在服务收到发货信息后的两周内包裹未送达数据中心，驱动器也会转换为此状态。 |
| 转移 | 服务开始将数据从驱动器传输到 Azure 存储时，驱动器会转为 Transferring 状态。 |
| 已完成 | 服务成功传输所有数据且未出错时，驱动器会转换为 Completed 状态。
| CompletedMoreInfo | 如果服务在从驱动器中复制数据或将数据复制到驱动器时遇到一些问题，驱动器会转为 CompletedMoreInfo 状态。 信息可以包含有关覆盖 Blob 的错误、警告或信息性消息。
| ShippedBack | 驱动器从数据中心寄送到回邮地址后，驱动器将转换为 ShippedBack 状态。 |

Azure 门户中的此映像会显示示例作业的驱动器状态：

![查看驱动器状态](./media/storage-import-export-service/drivestate.png)

下表描述了驱动器故障状态以及针对每种状态采取的措施。

| 驱动器状态 | 事件 | 解决方法/后续步骤 |
|:--- |:--- |:--- |
| NeverReceived | 标记为 NeverReceived 的驱动器（因为在作业寄送过程中未收到）通过另一次寄送送达。 | 运营团队将驱动器状态转为 Received。 |
| 不适用 | 不属于任何作业的驱动器会作为其他作业的一部分送至数据中心。 | 完成与原始包裹关联的作业后，驱动器会标记为额外驱动器并寄回给客户。 |

### <a name="time-to-process-job"></a>处理作业的时间
处理导入/导出作业的时间各不相同，取决于很多因素，例如寄送时间、数据中心的加载、要复制的数据的作业类型和大小，以及作业中的磁盘数量。 导入/导出服务没有 SLA，但在收到磁盘之后，服务力求在 7 到 10 天内完成复制。 除了在 Azure 门户上发布的状态以外，REST API 还可用于跟踪作业进度。 作业操作 API 调用的列表中的完成百分比参数提供了复制进度的百分比。

### <a name="pricing"></a>定价
**驱动器处理费用**

在导入或导出作业的过程中，处理每个驱动器都需要支付驱动器处理费用。 请参阅有关 [Azure 导入/导出定价](https://www.azure.cn/pricing/details/storage-import-export/)的详细信息。

**寄送费用**

将驱动器寄送到 Azure 时，需要向快递商支付寄送费用。 当 Microsoft 将驱动器寄回给你时，寄送费用由你在创建作业时提供的快递商帐户支付。

**事务成本**

将数据导入 Azure 存储时，除标准存储事务成本外没有任何事务成本。 将数据从 Blob 存储导出时，需支付标准的传出费用。 有关事务费用的更多详细信息，请参阅 [数据传输定价](https://www.azure.cn/pricing/details/data-transfer/)



## <a name="how-to-import-data-into-azure-file-storage-using-internal-sata-hdds-and-ssds"></a>如何使用内部 SATA HDD 和 SSD 将数据导入 Azure 文件存储？
如果将磁盘上的数据导入 Azure 文件 存储，请执行以下步骤。
使用 Azure 导入/导出服务导入数据时，第一步是通过 WAImportExport 工具准备驱动器。 按照以下步骤准备驱动器。

1. 标识要导入 Azure 文件存储的数据。 导入的数据可以是本地服务器或网络共享中的目录和独立文件。  
2. 根据数据总大小确定所需驱动器数目。 采购所需数目的 2.5 英寸 SSD 或者 2.5/3.5 英寸 SATA II 或 III 硬盘驱动器。
4. 确定要复制到每个磁盘驱动器的目录和/或独立文件。
5. 为数据集和驱动器集创建 CSV 文件。
    
  以下是导入数据作为 Azure 文件的数据集 CSV 文件示例：
  
    ```
    BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
    "F:\50M_original\100M_1.csv.txt","fileshare/100M_1.csv.txt",file,rename,"None",None
    "F:\50M_original\","fileshare/",file,rename,"None",None 
    ```
   In the above example, 100M_1.csv.txt  will be copied to the root of the "fileshare". If the "Fileshare" does not exist, one will be created. All files and folders under 50M_original will be recursively copied to fileshare. Folder structure will be maintained.

    Learn more about [preparing the dataset CSV file](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    


    **Driveset CSV File**

    The value of the driveset flag is a CSV file which contains the list of disks to which the drive letters are mapped in order for the tool to correctly pick the list of disks to be prepared. 

    Below is the example of driveset CSV file:
    
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey  G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |  H,Format,SilentMode,Encrypt,
    ```

    In the above example, it is assumed that two disks are attached and basic NTFS volumes with volume-letter G:\ and H:\ have been created. The tool will format and encrypt the disk which hosts H:\ and will not format or encrypt the disk hosting volume G:\.

    Learn more about [preparing the driveset CSV file](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file).

6.  Use the [WAImportExport Tool](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) to copy your data to one or more hard drives.
7.  You can specify "Encrypt" on Encryption field in drivset CSV to enable BitLocker encryption on the hard disk drive. Alternatively, you could also enable BitLocker encryption manually on the hard disk drive and specify "AlreadyEncrypted" and supply the key in the driveset CSV while running the tool.

8. Do not modify the data on the hard disk drives or the journal file after completing disk preparation.

> [!IMPORTANT]
> Each hard disk drive you prepare will result in a journal file. When you are creating the import job using the Azure portal, you must upload all the journal files of the drives which are part of that import job. Drives without journal files will not be processed.
> 
>

Below are the commands and examples for preparing the hard disk drive using WAImportExport tool.

WAImportExport tool PrepImport command for the first copy session to copy directories and/or files with a new copy session:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**Import example 1**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

In order to **add more drives**, one can create a new driveset file and run the command as below. For subsequent copy sessions to the different disk drives than specified in InitialDriveset .csv file, specify a new driveset CSV file and provide it as a value to the parameter "AdditionalDriveSet". Use the **same journal file** name and provide a **new session ID**. The format of AdditionalDriveset CSV file is same as InitialDriveSet format.

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
```

**Import example 2**
```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
```

In order to add additional data to the same driveset, WAImportExport tool PrepImport command can be called for subsequent copy sessions to copy additional files/directory:
For subsequent copy sessions to the same hard disk drives specified in InitialDriveset .csv file, specify the **same journal file** name and provide a **new session ID**; there is no need to provide the storage account key.

```
WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
```

**Import example 3**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

See more details about using the WAImportExport tool in [Preparing hard drives for import](storage-import-export-tool-preparing-hard-drives-import.md).

Also, refer to the [Sample workflow to prepare hard drives for an import job](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md) for more detailed step-by-step instructions.  



## Create an export job
Create an export job to notify the Import/Export service that you'll be shipping one or more empty drives to the data center so that data can be exported from your storage account to the drives and the drives then shipped to you.

### Prepare your drives
Following pre-checks are recommended for preparing your drives for an export job:

1. Check the number of disks required using the WAImportExport tool's PreviewExport command. For more information, see [Previewing Drive Usage for an Export Job](https://msdn.microsoft.com/library/azure/dn722414.aspx). It helps you preview drive usage for the blobs you selected, based on the size of the drives you are going to use.
2. Check that you can read/write to the hard drive that will be shipped for the export job.

### Create the export job
1. To create an export job, navigate to More services -> STORAGE -> "Import/export jobs" on the Azure portal. Click **Create Import/export Job**.
2. In Step 1 Basics, select "Export from Azure", enter a string for job name, select a subscription, enter or select a resource group. Enter a descriptive name for the import job. Note that the name you enter may contain only lowercase letters, numbers, hyphens, and underscores, must start with a letter, and may not contain spaces. You will use the name you choose to track your jobs while they are in progress and once they are completed. provide contact information for the person responsible for this export job. 

3. In Step 2 Job details, select the storage account that the data will be exported from in the Storage account section. The Drop-Off location  will be automatically populated based on the region of the storage account selected. Specify which blob data you wish to export from your storage account to your blank drive or drives. You can choose to export all blob data in the storage account, or you can specify which blobs or sets of blobs to export.
   
   To specify a blob to export, use the **Equal To** selector, and specify the relative path to the blob, beginning with the container name. Use *$root* to specify the root container.
   
   To specify all blobs starting with a prefix, use the **Starts With** selector, and specify the prefix, beginning with a forward slash '/'. The prefix may be the prefix of the container name, the complete container name, or the complete container name followed by the prefix of the blob name.
   
   The following table shows examples of valid blob paths:
   
   | Selector | Blob Path | Description |
   | --- | --- | --- |
   | Starts With |/ |Exports all blobs in the storage account |
   | Starts With |/$root/ |Exports all blobs in the root container |
   | Starts With |/book |Exports all blobs in any container that begins with prefix **book** |
   | Starts With |/music/ |Exports all blobs in container **music** |
   | Starts With |/music/love |Exports all blobs in container **music** that begin with prefix **love** |
   | Equal To |$root/logo.bmp |Exports blob **logo.bmp** in the root container |
   | Equal To |videos/story.mp4 |Exports blob **story.mp4** in container **videos** |
   
   You must provide the blob paths in valid formats to avoid errors during processing, as shown in this screenshot.
   
   ![Create export job - Step 3](./media/storage-import-export-service/export-job-03.png)

4. In Step 3 Return shipping info, select the carrier from the drop down list and enter a valid carrier account number that you have created with that carrier. We will use this account to ship the drives back to you once your import job is complete. Provide a complete and valid contact name, phone, email, street address, city, zip, state/proviince and country/region..
   
 5. In the Summary Page, Azure DataCenter shipping address is provided to be used for shipping disks to Azure DC. Ensure that the job name and the full address are mentioned on the shipping label. 

6. Click OK on the Summary Page to complete Import job creation

7. After shipping the disks, return to the **Import/Export** page on the Azure portal, 
     a) Navigate and click on the import job
     b) Click on **Update job status and tracking info once drives are shipped**. 
     c) Select the check box "Mark as shipped"
     d) Provide the Carrier and Tracking number.
    
   If the tracking number is not updated within 2 weeks of creating the job, the job will expire.
   
8. You can track your job progress on the portal dashboard. See what each job state in the previous section means by [Viewing your job status](#viewing-your-job-status).

   > [!NOTE]
   > If the blob to be exported is in use at the time of copying to hard drive, Azure Import/Export service will take a snapshot of the blob and copy the snapshot.
   > 
   > 
 
9. After you receive the drives with your exported data, you can view and copy the BitLocker keys generated by the service for your drive. Navigate to export job in the Azure portal and click the Import/Export tab. Select your export job from the list, and click the BitLocker Keys option. The BitLocker keys appear as shown below:
   
   ![View BitLocker keys for export job](./media/storage-import-export-service/export-job-bitlocker-keys.png)

Please go through the FAQ section below as it covers the most common questions customers encounter when using this service.

## Frequently asked questions

**Can I copy Azure File storage using the Azure Import/Export service?**

Yes, the Azure Import/Export service supports import to Azure File Storage. It does not support export of Azure Files at this time.

**Is the Azure Import/Export service available for CSP subscriptions?**

Azure Import/Export service does support CSP subscriptions.

**Can I skip the drive preparation step for an import job or can I prepare a drive without copying?**

Any drive that you want to ship for importing data must be prepared using the Azure WAImportExport tool. You must use the WAImportExport tool to copy data to the drive.

**Do I need to perform any disk preparation when creating an export job?**

No, but some pre-checks are recommended. Check the number of disks required using the WAImportExport tool's PreviewExport command. For more information, see [Previewing Drive Usage for an Export Job](https://msdn.microsoft.com/library/azure/dn722414.aspx). It helps you preview drive usage for the blobs you selected, based on the size of the drives you are going to use. Also check that you can read from and write to the hard drive that will be shipped for the export job.

**What happens if I accidentally send an HDD which does not conform to the supported requirements?**

The Azure data center will return the drive that does not conform to the supported requirements to you. If only some of the drives in the package meet the support requirements, those drives will be processed, and the drives that do not meet the requirements will be returned to you.

**Can I cancel my job?**

You can cancel a job when its status is Creating or Shipping.

**How long can I view the status of completed jobs in the Azure portal?**

You can view the status for completed jobs for up to 90 days. Completed jobs will be deleted after 90 days.

**If I want to import or export more than 10 drives, what should I do?**

One import or export job can reference only 10 drives in a single job for the Import/Export service. If you want to ship more than 10 drives, you can create multiple jobs. Drives that are associated with the same job must be shipped together in the same package.

**Does the service format the drives before returning them?**

No. All drives are encrypted with BitLocker.

**Can I purchase drives for import/export jobs from 21Vianet?**

No. You will need to ship your own drives for both import and export jobs.

** How can I access data that is imported by this service**

The data under your Azure storage account can be accessed via the Azure Portal or using a standalone tool called Storage Explorer. https://docs.azure.cn/vs-azure-tools-storage-manage-with-storage-explorer 

**After the import job completes, what will my data look like in the storage account? Will my directory hierarchy be preserved?**

When preparing a hard drive for an import job, the destination is specified by the DstBlobPathOrPrefix field in the dataset CSV. This is the destination container in the storage account to which data from the hard drive is copied. Within this destination container, virtual directories are created for folders from the hard drive and blobs are created for files. 

**If the drive has files that already exist in my storage account, will the service overwrite existing blobs or files in my storage account?**

When preparing the drive, you can specify whether the destination files should be overwritten or ignored using the field in dataset CSV file called Disposition:<rename|no-overwrite|overwrite>. By default, the service will rename the new files rather than overwrite existing blobs or files.

**Is the WAImportExport tool compatible with 32-bit operating systems?**
No. The WAImportExport tool is only compatible with 64-bit Windows operating systems. Please refer to the Operating Systems section in the [pre-requisites](#pre-requisites) for a complete list of supported OS versions.

**Should I include anything other than the hard disk drive in my package?**

Please ship only your hard drives. Do not include items like power supply cables or USB cables.

**Do I have to ship my drives using FedEx or China Postal Service?**

You can ship drives to the data center using any known carrier like FedEx or China Postal Service. However, for shipping the drives back to you from the data center, you must provide a available address.

**Are there any restrictions with shipping my drive internationally?**

Please note that the physical media that you are shipping may need to cross international borders. You are responsible for ensuring that your physical media and data are imported and/or exported in accordance with the applicable laws. Before shipping the physical media, check with your advisors to verify that your media and data can legally be shipped to the identified data center. This will help to ensure that it reaches 21Vianet in a timely manner.

**When creating a job, the shipping address is a location that is different from my storage account location. What should I do?**

Some storage account locations are mapped to alternate shipping locations. Previously available shipping locations can also be temporarily mapped to alternate locations. Always check the shipping address provided during job creation before shipping your drives.

**When shipping my drive, the carrier asks for the data center contact address and phone number. What should I provide?**

The phone number and DC address is provided as part of job creation.

**Can I use the Azure Import/Export service to copy PST mailboxes and SharePoint data to O365?**

Please refer to [Import PST files or SharePoint data to Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Can I use the Azure Import/Export service to copy my backups offline to the Azure Backup Service?**

Please refer to [Offline Backup workflow in Azure Backup](../../backup/backup-azure-backup-import-export.md).

**What is the maximum number of HDD for in one shipment?**

Any number of HDDs can be in one shipment and if the disks belong to multiple jobs it is recommended to 
a) Have the disks labeled with the corresponding job names. 
b) Update the jobs with a tracking number suffixed with -1, -2 etc.
  
**What is the Maximum Block Blob and Page Blob Size supported by Disk Import/Export?**

Max Block Blob size is approximately 4.768TB  or 5,000,000 MB.
Max Page Blob size is 1TB.

**Does Disk Import/Export support AES 256 encryption?**

Azure Import/Export service by default encrypts with AES 128 bitlocker encryption but this can be increased to AES 256 by manually encrypting with bitlocker before data is copied. 

If using [WAImportExport V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), below is a sample command
```
WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
```
If using [WAImportExport Tool](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) specify "AlreadyEncrypted" and supply the key in the driveset CSV.
```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
```
## Next steps

* [Setting up the WAImportExport tool](storage-import-export-tool-how-to.md)
* [Transfer data with the AzCopy command-line utility](storage-use-azcopy.md)
* [Azure Import Export REST API sample](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/)

<!--Update_Description: wording update-->