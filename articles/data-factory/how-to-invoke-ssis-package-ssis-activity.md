---
title: 使用“执行 SSIS 包”活动运行 SSIS 包 - Azure | Microsoft Docs
description: 本文介绍如何使用“执行 SSIS 包”活动在 Azure 数据工厂管道中运行 SQL Server Integration Services (SSIS) 包。
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 11/11/2019
author: WenJason
ms.author: v-jay
ms.reviewer: douglasl
manager: digimobile
ms.openlocfilehash: cb77ec4c5eb9b76bafaa4cc32de9f1cc6d9d07a9
ms.sourcegitcommit: ff8dcf27bedb580fc1fcae013ae2ec28557f48ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2019
ms.locfileid: "73648761"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>在 Azure 数据工厂中使用“执行 SSIS 包”活动运行 SSIS 包
本文介绍如何使用“执行 SSIS 包”活动在 Azure 数据工厂管道中运行 SQL Server Integration Services (SSIS) 包。 

## <a name="prerequisites"></a>先决条件

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

如果还没有 Azure-SSIS Integration Runtime (IR)，请按照以下文章中的分步说明创建 IR：[教程：预配 Azure-SSIS IR](tutorial-create-azure-ssis-runtime-portal.md)。

## <a name="run-a-package-in-the-azure-portal"></a>在 Azure 门户中运行包
在本部分中，我们将使用数据工厂用户界面 (UI) 或应用来创建一个数据工厂管道，其中包含可运行 SSIS 包的“执行 SSIS 包”活动。

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>使用“执行 SSIS 包”活动创建管道
此步骤使用数据工厂 UI 或应用创建管道。 将“执行 SSIS 包”活动添加到管道，并将该活动配置为运行 SSIS 包。 

1. 在 Azure 门户中的数据工厂概述或主页上，选择“创作和监视”磁贴，在单独的选项卡中启动数据工厂 UI 或应用。  

   ![数据工厂主页](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)

   在“开始使用”页中，选择“创建管道”。   

   ![“入门”页](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)

1. 在“活动”  工具箱中，展开“常规”  。 然后将“执行 SSIS 包”活动拖到管道设计图面上。  

   ![将“执行 SSIS 包”活动拖到设计面](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

1. 在“执行 SSIS 包”活动的“常规”选项卡上，提供活动的名称和说明  。 设置可选的“超时”和“重试”值。  

   ![在“常规”选项卡上设置属性](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

1. 在“执行 SSIS 包”活动的“设置”选项卡上，选择要在其中运行包的 Azure-SSIS IR。  如果包使用 Windows 身份验证访问数据存储（例如本地的 SQL Server 或文件共享，或 Azure 文件存储），请选中“Windows 身份验证”复选框。  在“域”、“用户名”和“密码”框中输入包执行凭据的值。    

    或者，可以将 Azure Key Vault 中存储的机密用作其值。 为此，请选中相关凭据旁边的“AZURE KEY VAULT”复选框。  选择或编辑现有的 Key Vault 链接服务，或创建新的链接服务。 然后为凭据值选择机密名称或版本。

    创建或编辑 Key Vault 链接服务时，可以选择或编辑现有的 Key Vault，或创建新的 Key Vault。 请务必授予数据工厂托管标识对 Key Vault 的访问权限（如果尚未这样做）。 此外，还可以采用以下格式直接输入机密：`<Key vault linked service name>/<secret name>/<secret version>`。 如果包需要 32 位运行时才能运行，请选中“32 位运行时”复选框  。

   对于“包位置”，请选择“SSISDB”、“文件系统(包)”或“文件系统(项目)”。     如果选择“SSISDB”作为包位置（如果为 Azure-SSIS IR 预配了 Azure SQL 数据库服务器或托管实例托管的 SSIS 目录 (SSISDB)，则会自动选择该选项），请指定要运行的、已部署到 SSISDB 中的包。  

    如果 Azure-SSIS IR 正在运行且清除了“手动输入内容”复选框，可以从 SSISDB 浏览并选择现有的文件夹、项目、包或环境  。 选择“刷新”以从 SSISDB 获取新添加的文件夹、项目、包或环境，以便可以浏览和选择这些内容  。 若要浏览或选择包执行的环境，必须事先配置项目，以便从 SSISDB 下的相同文件夹中添加这些环境作为引用。 有关详细信息，请参阅[创建和映射 SSIS 环境](https://docs.microsoft.com/sql/integration-services/create-and-map-a-server-environment?view=sql-server-2014)。

   对于“日志记录级别”，请为包执行选择预定义的日志记录范围  。 如果要改为输入自定义日志记录名称，请选中“自定义”复选框  。 

   ![在“设置”选项卡上设置属性 - 自动](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

   如果 Azure-SSIS IR 未在运行或“手动输入内容”  复选框处于选中状态，请采用以下格式直接在 SSISDB 中输入你的包和环境路径：`<folder name>/<project name>/<package name>.dtsx` 和 `<folder name>/<environment name>`。

   ![在“设置”选项卡上设置属性 - 手动](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings2.png)

   如果选择“文件系统(包)”作为包位置（如果未为 Azure-SSIS IR 预配 SSISDB，则会自动选择该选项），请通过在“包路径”框中提供包文件 (`.dtsx`) 的通用命名约定 (UNC) 路径来指定要运行的包。   例如，如果将包存储在 Azure 文件存储中，则其包路径为 `\\<storage account name>.file.core.chinacloudapi.cn\<file share name>\<package name>.dtsx`。 
   
   如果在单独的文件中配置包，则还需要在“配置路径”框中提供配置文件 (`.dtsConfig`) 的 UNC 路径。  例如，如果将配置存储在 Azure 文件存储中，则其配置路径为 `\\<storage account name>.file.core.chinacloudapi.cn\<file share name>\<configuration name>.dtsConfig`。

   ![在“设置”选项卡上设置属性 - 手动](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings3.png)

   如果选择“文件系统(项目)”作为包位置，请通过在“项目路径”框中提供项目文件 (`.ispac`) 的 UNC 路径，并在“包名称”框中提供项目中某个包文件 (`.dtsx`) 的 UNC 路径，来指定要运行的包。    例如，如果将项目存储在 Azure 文件存储中，则其项目路径为 `\\<storage account name>.file.core.chinacloudapi.cn\<file share name>\<project name>.ispac`。

   ![在“设置”选项卡上设置属性 - 手动](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings4.png)

   接下来，指定用于访问项目、包或配置文件的凭据。 如果先前已输入包执行凭据的值（参阅上文），则可以通过选中“与包执行凭据相同”复选框来重复使用这些值。  否则，请在“域”、“用户名”和“密码”框中输入包访问凭据的值。    例如，如果将项目、包或配置存储在 Azure 文件存储中，则域为 `Azure`，用户名为 `<storage account name>`，密码为 `<storage account key>`。 

   或者，可将 Key Vault 中存储的机密用作其值（参阅上文）。 这些凭据用于访问“执行包任务”中的包和子包，全部来自其自身的路径或相同的项目以及配置（包括包中指定的配置）。 
   
   如果在通过 SQL Server Data Tools 创建包时使用了 **EncryptAllWithPassword** 或 **EncryptSensitiveWithPassword** 保护级别，请在“加密密码”框中输入密码的值。  或者，可将 Key Vault 中存储的机密用作其值（参阅上文）。 如果使用了 **EncryptSensitiveWithUserKey** 保护级别，请在配置文件中或在“SSIS 参数”、“连接管理器”或“属性替代”选项卡上重新输入敏感值（参阅下文）。    

   不支持使用 **EncryptAllWithUserKey** 保护级别。 需要通过 SQL Server Data Tools 或 `dtutil` 命令行实用工具将包重新配置为使用另一保护级别。 
   
   对于“日志记录级别”，请为包执行选择预定义的日志记录范围  。 如果要改为输入自定义日志记录名称，请选中“自定义”复选框  。 若要记录包执行但不使用可在包中指定的标准日志提供程序，请通过在“日志记录路径”框中提供其 UNC 路径来指定日志文件夹。  例如，如果将日志存储在 Azure 文件存储中，则日志记录路径为 `\\<storage account name>.file.core.chinacloudapi.cn\<file share name>\<log folder name>`。 将在此路径中为每个包运行创建一个与“执行 SSIS 包”活动运行 ID 同名的子文件夹，其中的日志文件每隔 5 分钟生成一次。 
   
   最后，请指定用于访问日志文件夹的凭据。 如果先前已输入包访问凭据的值（参阅上文），则可以通过选中“与包访问凭据相同”复选框来重复使用这些值。  否则，请在“域”、“用户名”和“密码”框中输入日志记录访问凭据的值。    例如，如果将日志存储在 Azure 文件存储中，则域为 `Azure`，用户名为 `<storage account name>`，密码为 `<storage account key>`。 

    或者，可将 Key Vault 中存储的机密用作其值（参阅上文）。 这些凭据用于存储日志。 
   
   对于上述所有 UNC 路径，完全限定的文件名必须短于 260 个字符。 目录名称必须短于 248 个字符。

1. 在“执行 SSIS 包”活动的“SSIS 参数”选项卡上，如果 Azure-SSIS IR 正在运行、已选择“SSISDB”作为包位置，且已清除“设置”选项卡上的“手动输入内容”复选框，则会显示 SSISDB 中选定项目或包中现有的 SSIS 参数，以便为它们赋值     。 否则，可以逐个输入以便手动为它们赋值。 为了使包成功执行，请确保它们存在并已正确输入。 
   
   如果通过 SQL Server Data Tools 创建包时使用了 **EncryptSensitiveWithUserKey** 保护级别，且已选择“文件系统(包)”或“文件系统(项目)”作为包位置，则还需要重新输入敏感参数，以便在配置文件或此选项卡上为它们赋值。   
   
   为参数赋值时，可以使用表达式、函数、数据工厂系统变量和数据工厂管道参数或变量添加动态内容。 或者，可将 Key Vault 中存储的机密用作其值（参阅上文）。

   ![在“SSIS 参数”选项卡上设置属性](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-ssis-parameters.png)

1. 在用于“执行 SSIS 包”活动的“连接管理器”选项卡上，如果 Azure-SSIS IR 正在运行、已选择“SSISDB”作为包位置，且已清除“设置”选项卡上的“手动输入内容”复选框，则 SSISDB 中的选定项目或包中的现有连接管理器将会显示，供你为其属性赋值     。 否则，可以逐个输入以便手动为其属性赋值。 为了使包成功执行，请确保它们存在并已正确输入。 
   
   如果通过 SQL Server Data Tools 创建包时使用了 **EncryptSensitiveWithUserKey** 保护级别，且已选择“文件系统(包)”或“文件系统(项目)”作为包位置，则还需要重新输入敏感的连接管理器属性，以便在配置文件或此选项卡上为它们赋值。   
   
   为连接管理器属性赋值时，可以使用表达式、函数、数据工厂系统变量和数据工厂管道参数或变量添加动态内容。 或者，可将 Key Vault 中存储的机密用作其值（参阅上文）。

   ![在“连接管理器”选项卡上设置属性](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-connection-managers.png)

1. 在“执行 SSIS 包”活动的“属性替代”选项卡上，逐个输入选定包的现有属性的路径，以便手动为其赋值。  为了使包成功执行，请确保它们存在并已正确输入。 例如，若要替代用户变量的值，请按以下格式输入其路径：`\Package.Variables[User::<variable name>].Value`。 
   
   如果通过 SQL Server Data Tools 创建包时使用了 **EncryptSensitiveWithUserKey** 保护级别，且已选择“文件系统(包)”或“文件系统(项目)”作为包位置，则还需要重新输入敏感属性，以便在配置文件或此选项卡上为它们赋值。   
   
   为属性赋值时，可以使用表达式、函数、数据工厂系统变量和数据工厂管道参数或变量添加动态内容。

   ![在“属性替代”选项卡上设置属性](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-property-overrides.png)

   可以使用“连接管理器”或“属性替代”选项卡替代配置文件中和“SSIS 参数”选项卡上的赋值。    还可以使用“属性替代”选项卡替代“连接管理器”选项卡上的赋值。  

1. 若要验证管道配置，请在工具栏中选择“验证”  。 若要关闭“管道验证报告”，  请选择 **>>** 。

1. 若要将管道发布到数据工厂，请选择“全部发布”  。 

### <a name="run-the-pipeline"></a>运行管道
在此步骤中，将触发管道运行。 

1. 若要触发某个管道运行，请在工具栏中选择“触发器”  ，然后选择“立即触发”  。 

   ![立即触发](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. 在“管道运行”窗口中选择“完成”。   

### <a name="monitor-the-pipeline"></a>监视管道

1. 在左侧切换到“监视”选项卡。  随即显示管道运行及其状态和其他信息（例如“运行开始”时间）。  若要刷新视图，请选择“刷新”。 

   ![管道运行](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

2. 在“操作”列中选择“查看活动运行”链接。   此时只显示一个活动运行，因为管道只有一个活动。 该活动为“执行 SSIS 包”活动。

   ![活动运行](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

3. 在 SQL Server 中针对 SSISDB 数据库运行以下查询，验证是否执行了该包。 

   ```sql
   select * from catalog.executions
   ```

   ![验证包执行](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

4. 还可以从管道活动运行的输出中获取 SSISDB 执行 ID，并使用此 ID 在 SQL Server Management Studio 中查看更全面的执行日志和错误消息。

   ![获取执行 ID。](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

### <a name="schedule-the-pipeline-with-a-trigger"></a>使用触发器计划管道

还可以为管道创建一个计划触发器，以便按计划（例如每小时或每天）运行管道。 有关示例，请参阅[创建数据工厂 - 数据工厂 UI](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule)。

## <a name="run-a-package-with-powershell"></a>使用 PowerShell 运行包
在此部分中，将使用 Azure PowerShell 创建一个数据工厂管道，管道中包含可运行 SSIS 包的“执行 SSIS 包”活动。 

按照[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) 中的分步说明安装最新的 Azure PowerShell 模块。

### <a name="create-a-data-factory-with-azure-ssis-ir"></a>创建包含 Azure-SSIS IR 的数据工厂
可以使用已预配 Azure-SSIS IR 的现有数据工厂，或者创建包含 Azure-SSIS IR 的新数据工厂。 按照[教程：通过 PowerShell 将 SSIS 包部署到 Azure](/data-factory/tutorial-deploy-ssis-packages-azure-powershell) 中的分步说明，创建包含 Azure-SSIS IR 的新 ADF。

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>使用“执行 SSIS 包”活动创建管道 
在此步骤中创建包含“执行 SSIS 包”活动的管道。 该活动运行 SSIS 包。 

1. 在 *C:\ADF\RunSSISPackage* 文件夹中创建名为 *RunSSISPackagePipeline.json* 的 JSON 文件，并在其中包含类似于以下示例的内容。

   > [!IMPORTANT]
   > 在保存该文件之前，请替换对象名称、说明、路径、属性或参数值、密码及其他变量值。 
    
   ```json
   {
       "name": "RunSSISPackagePipeline",
       "properties": {
           "activities": [{
               "name": "MySSISActivity",
               "description": "My SSIS package/activity description",
               "type": "ExecuteSSISPackage",
               "typeProperties": {
                   "connectVia": {
                       "referenceName": "MyAzureSSISIR",
                       "type": "IntegrationRuntimeReference"
                   },
                   "executionCredential": {
                       "domain": "MyExecutionDomain",
                       "username": "MyExecutionUsername",
                       "password": {
                           "type": "SecureString",
                           "value": "MyExecutionPassword"
                       }
                   },
                   "runtime": "x64",
                   "loggingLevel": "Basic",
                   "packageLocation": {
                       "packagePath": "MyFolder/MyProject/MyPackage.dtsx",
                       "type": "SSISDB"
                   },
                   "environmentPath": "MyFolder/MyEnvironment",
                   "projectParameters": {
                       "project_param_1": {
                           "value": "123"
                       },
                       "project_param_2": {
                           "value": {
                               "value": "@pipeline().parameters.MyProjectParameter",
                               "type": "Expression"
                           }
                       }
                   },
                   "packageParameters": {
                       "package_param_1": {
                           "value": "345"
                       },
                       "package_param_2": {
                           "value": {
                               "type": "AzureKeyVaultSecret",
                               "store": {
                                   "referenceName": "myAKV",
                                   "type": "LinkedServiceReference"
                               },
                               "secretName": "MyPackageParameter"
                           }
                       }
                   },
                   "projectConnectionManagers": {
                       "MyAdonetCM": {
                           "username": {
                               "value": "MyConnectionUsername"
                           },
                           "password": {
                               "value": {
                                   "type": "SecureString",
                                   "value": "MyConnectionPassword"
                               }
                           }
                       }
                   },
                   "packageConnectionManagers": {
                       "MyOledbCM": {
                           "username": {
                               "value": {
                                   "value": "@pipeline().parameters.MyConnectionUsername",
                                   "type": "Expression"
                               }
                           },
                           "password": {
                               "value": {
                                   "type": "AzureKeyVaultSecret",
                                   "store": {
                                       "referenceName": "myAKV",
                                       "type": "LinkedServiceReference"
                                   },
                                   "secretName": "MyConnectionPassword",
                                   "secretVersion": "MyConnectionPasswordVersion"
                               }
                           }
                       }
                   },
                   "propertyOverrides": {
                       "\\Package.MaxConcurrentExecutables": {
                           "value": 8,
                           "isSensitive": false
                       }
                   }
               },
               "policy": {
                   "timeout": "0.01:00:00",
                   "retry": 0,
                   "retryIntervalInSeconds": 30
               }
           }]
       }
   }
   ```

   若要执行存储在文件系统、文件共享或 Azure 文件存储中的包，请输入包或日志位置属性的值，如下所示：

   ```json
   {
       {
           {
               {
                   "packageLocation": {
                       "packagePath": "//MyStorageAccount.file.core.chinacloudapi.cn/MyFileShare/MyPackage.dtsx",
                       "type": "File",
                       "typeProperties": {
                           "packagePassword": {
                               "type": "SecureString",
                               "value": "MyEncryptionPassword"
                           },
                           "accessCredential": {
                               "domain": "Azure",
                               "username": "MyStorageAccount",
                               "password": {
                                   "type": "SecureString",
                                   "value": "MyAccountKey"
                               }
                           }
                       }
                   },
                   "logLocation": {
                       "logPath": "//MyStorageAccount.file.core.chinacloudapi.cn/MyFileShare/MyLogFolder",
                       "type": "File",
                       "typeProperties": {
                           "accessCredential": {
                               "domain": "Azure",
                               "username": "MyStorageAccount",
                               "password": {
                                   "type": "AzureKeyVaultSecret",
                                   "store": {
                                       "referenceName": "myAKV",
                                       "type": "LinkedServiceReference"
                           },
                                   "secretName": "MyAccountKey"
                               }
                           }
                       }
                   }
               }
           }
       }
   }
   ```

   若要执行存储在文件系统、文件共享或 Azure 文件存储中项目中的包，请输入包位置属性的值，如下所示：

   ```json
   {
       {
           {
               {
                   "packageLocation": {
                       "packagePath": "//MyStorageAccount.file.core.chinacloudapi.cn/MyFileShare/MyProject.ispac:MyPackage.dtsx",
                       "type": "File",
                       "typeProperties": {
                           "packagePassword": {
                               "type": "SecureString",
                               "value": "MyEncryptionPassword"
                           },
                           "accessCredential": {
                               "domain": "Azure",
                               "userName": "MyStorageAccount",
                               "password": {
                                   "type": "SecureString",
                                   "value": "MyAccountKey"
                               }
                           }
                       }
                   }
               }
           }
       }
   }
   ```

2. 在 Azure PowerShell 中，切换到 *C:\ADF\RunSSISPackage* 文件夹。

3. 若要创建管道 **RunSSISPackagePipeline**，请运行 **Set-AzDataFactoryV2Pipeline** cmdlet。

   ```powershell
   $DFPipeLine = Set-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                  -ResourceGroupName $ResGrp.ResourceGroupName `
                                                  -Name "RunSSISPackagePipeline"
                                                  -DefinitionFile ".\RunSSISPackagePipeline.json"
   ```

   下面是示例输出：

   ```
   PipelineName      : Adfv2QuickStartPipeline
   ResourceGroupName : <resourceGroupName>
   DataFactoryName   : <dataFactoryName>
   Activities        : {CopyFromBlobToBlob}
   Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
   ```

### <a name="run-the-pipeline"></a>运行管道
使用 **Invoke-AzDataFactoryV2Pipeline** cmdlet 运行该管道。 此 cmdlet 返回管道运行 ID，用于将来的监视。

```powershell
$RunId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline"></a>监视管道

运行以下 PowerShell 脚本，持续检查管道运行状态，直到完成数据复制为止。 在 PowerShell 窗口中复制或粘贴以下脚本，然后按 Enter。 

```powershell
while ($True) {
    $Run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName `
                                               -DataFactoryName $DataFactory.DataFactoryName `
                                               -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

还可使用 Azure 门户监视管道。 有关分步说明，请参阅[监视管道](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)。

### <a name="schedule-the-pipeline-with-a-trigger"></a>使用触发器计划管道
在上一步骤中，已按需运行了管道。 还可创建一个计划触发器，按计划（例如每小时或每天）运行管道。

1. 在 *C:\ADF\RunSSISPackage* 文件夹中创建名为 *MyTrigger.json* 的 JSON 文件，并在其中包含以下内容： 
        
   ```json
   {
       "properties": {
           "name": "MyTrigger",
           "type": "ScheduleTrigger",
           "typeProperties": {
               "recurrence": {
                   "frequency": "Hour",
                   "interval": 1,
                   "startTime": "2017-12-07T00:00:00-08:00",
                   "endTime": "2017-12-08T00:00:00-08:00"
               }
           },
           "pipelines": [{
               "pipelineReference": {
                   "type": "PipelineReference",
                   "referenceName": "RunSSISPackagePipeline"
               },
               "parameters": {}
           }]
       }
   }    
   ```
    
1. 在 Azure PowerShell 中，切换到 *C:\ADF\RunSSISPackage* 文件夹。
1. 运行 **Set-AzDataFactoryV2Trigger** cmdlet，以创建触发器。 

   ```powershell
   Set-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                   -DataFactoryName $DataFactory.DataFactoryName `
                                   -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
   ```
1. 默认情况下，触发器处于停止状态。 运行 **Start-AzDataFactoryV2Trigger** cmdlet 以启动该触发器。 

   ```powershell
   Start-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                     -DataFactoryName $DataFactory.DataFactoryName `
                                     -Name "MyTrigger" 
   ```
1. 通过运行 **Get-AzDataFactoryV2Trigger** cmdlet 确认该触发器已启动。 

   ```powershell
   Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                   -DataFactoryName $DataFactoryName `
                                   -Name "MyTrigger"     
   ```    
1. 在下一小时后运行以下命令。 例如，如果当前时间为下午 3:25 (UTC)，则在下午 4:00 (UTC) 运行该命令。 
    
   ```powershell
   Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                      -DataFactoryName $DataFactoryName `
                                      -TriggerName "MyTrigger" `
                                      -TriggerRunStartedAfter "2017-12-06" `
                                      -TriggerRunStartedBefore "2017-12-09"
   ```

   在 SQL Server 中针对 SSISDB 数据库运行以下查询，验证是否执行了该包。 

   ```sql
   select * from catalog.executions
   ```

