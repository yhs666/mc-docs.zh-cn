---
title: "连接器版本发行历史记录 | Microsoft Docs"
description: "本主题列出了 Forefront Identity Manager (FIM) 和 Microsoft Identity Manager (MIM) 的连接器的所有版本"
services: active-directory
documentationcenter: 
author: alexchen2016
manager: digimobile
editor: 
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
origin.date: 08/18/2017
ms.date: 08/24/2017
ms.author: v-junlch
ms.openlocfilehash: 3cae3d5015554e4202bb738fb128b1d2268a09b8
ms.sourcegitcommit: 0f2694b659ec117cee0110f6e8554d96ee3acae8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2017
---
# <a name="connector-version-release-history"></a>连接器版本发行历史记录
Forefront Identity Manager (FIM) 和 Microsoft Identity Manager (MIM) 的连接器会经常更新。

> [!NOTE]
> 本主题仅适用于 FIM 和 MIM。 不支持将这些连接器安装在 Azure AD Connect 上。 升级到指定的版本时，已发布的连接器会预安装在 AADConnect 上。

本主题列出所有已发布的连接器版本。

相关链接：

- [下载最新连接器](http://go.microsoft.com/fwlink/?LinkId=717495)
- [泛型 LDAP 连接器](active-directory-aadconnectsync-connector-genericldap.md)参考文档
- [泛型 SQL 连接器](active-directory-aadconnectsync-connector-genericsql.md)参考文档
- [Web 服务连接器](http://go.microsoft.com/fwlink/?LinkID=226245) 参考文档
- [PowerShell 连接器](active-directory-aadconnectsync-connector-powershell.md)参考文档
- [Lotus Domino 连接器](active-directory-aadconnectsync-connector-domino.md)参考文档


## <a name="116040-aadconnect-11xxx0"></a>1.1.604.0 (AADConnect 1.1.XXX.0)


### <a name="fixed-issues"></a>已解决的问题：

- 泛型 Web 服务：
  - 解决了存在两个或更多终结点时无法创建 SOAP 项目的问题。
- 泛型 SQL：
  - 在导入操作中，GSQL 在保存到连接器空间时未正确转换时间。 GSQL 连接器空间的默认日期和时间格式已从“yyyy-MM-dd hh:mm:ssZ”更改为“yyyy-MM-dd HH:mm:ssZ”。

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>已解决的问题：

- 泛型 Web 服务：
  - Wsconfig 工具未正确转换 REST 服务方法的“示例请求”中的 Json 数组。 这导致序列化 REST 请求的此 Json 数组时出现问题。
  - Web 服务连接器配置工具不支持在 JSON 属性名称中使用空间符号 
    - 可将替换模式手动添加到 WSConfigTool.exe.config 文件，例如 ```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```

- Lotus Notes：
  - 选项“允许组织/组织单位的自定义认证者”处于禁用状态时，导出（更新）期间连接将失败。导出流后，将所有属性导出到 Domino，但在导出时，KeyNotFoundException 会返回到同步。 
    - 这是因为重命名操作试图通过更改下列属性之一来更改 DN（用户名属性）时操作失败：  
      - LastName
      - FirstName
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - ou
      - altcommonname

  - “允许组织/组织单位的自定义认证者”选项已启用，但所需认证者仍为空时，会发生 KeyNotFoundException。

### <a name="enhancements"></a>增强功能：

- 泛型 SQL：
  - **方案：已重新实施：**“*”功能
  - **解决方案说明：**已更改[多值引用属性处理](active-directory-aadconnectsync-connector-genericsql.md)的方法。


### <a name="fixed-issues"></a>已解决的问题：

- 泛型 Web 服务：
  - 如果存在 WebService 连接器，则无法导入服务器配置
  - WebService 连接器不适用于多个 Web 服务

- 泛型 SQL：
  - 未为单值引用属性列出对象类型
  - 从多值表中删除值时，根据更改跟踪策略执行的增量导入将删除对象
  - 在 AS/400 上与 DB2 一起使用时，GSQL 连接器中会引发 OverflowException

Lotus：
  - 打开 GlobalParameters 页之前，添加启用\禁用搜索 OU 的选项

## <a name="114430"></a>1.1.443.0

发布时间：2017 年 3 月

### <a name="enhancements"></a>增强功能

- 泛型 SQL：</br>
  **情景症状：**我们仅允许引用一个对象类型，并要求对成员使用交叉引用，这是一个已知的 SQL 连接器限制。 </br>
  **解决方法说明：**如果选择了“*”选项，在执行引用的处理步骤时，对象类型的所有组合将返回给同步引擎。

>[!Important]
- 这就会创建许多的占位符
- 必须确保命名跨对象类型唯一。


- 泛型 LDAP：</br>
 **情景：**在特定的分区中只选择少量的容器时，搜索仍会针对整个分区执行。 具体的信息由同步服务而不是 MA 筛选，这可能会导致性能下降。 </br>

 **解决方案说明：** 更改了 GLDAP 连接器的代码，以便浏览所有容器，在每个容器中搜索对象，不必在整个分区进行搜索。


- Lotus Domino：

  **情景：**导出期间用于删除人员的 Domino 邮件删除支持。 </br>
  **解决方法：**导出期间可配置用于删除人员的 Domino 邮件删除支持。

### <a name="fixed-issues"></a>已解决的问题：
- 泛型 Web 服务：
 - 通过 WebService 配置工具在默认 SAP wsconfig 项目中更改服务 URL 时，会发生以下错误：找不到部分路径

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

- 泛型 LDAP：
 - GLDAP 连接器检测不到 AD LDS 中的所有属性
 - 从 LDAP 目录架构中检测不到 UPN 属性时，向导中断
 - 未选择“objectclass”属性时，增量导入失败，但完全导入期间并未出现发现错误
 - “配置分区和层次结构”配置页未显示类型与泛型 LDAP MA 中 Novel 服务器分区类型相同的  
任何对象， 而只显示了 RootDSE 分区中的对象。


- 泛型 SQL：
 - 对无法导入泛型 SQL 水印增量导入多值属性的 Bug 的修复
 - 导出多值属性的已删除/已添加值时，这些值未在数据源中删除/添加。  


- Lotus Notes：
 - 特定字段“全名”在 Metaverse 中显示正确，但在导出到 Notes 时，属性的值为 Null 或空。
 - 修复了“认证者重复”错误
 - 如果在 Lotus Domino 连接器上选择了没有任何数据的对象以及其他对象，则在执行完全导入时会收到发现错误。
 - 如果在 Lotus Domino 连接器上运行增量导入，则在运行结束时，Microsoft.IdentityManagement.MA.LotusDomino.Service.exe 服务有时会返回应用程序错误。
 - 组成员身份总体运行正常，并且可以进行保留，但在通过运行导出操作尝试从成员身份中删除用户时，会显示更新成功，但用户并未从 Lotus Notes 的成员身份中实际删除。
 - 在 Lotus MA 的配置 GUI 中添加了一个选项，允许选择“追加底部的项”作为导出模式，以便在多值属性的导出过程中追加底部的新项。
 - 连接器会添加所需的逻辑，以便从邮件文件夹和 ID 保管库中删除文件。
 - 删除不适用于跨 NAB 成员的成员身份。
 - 应可从多值属性中成功删除值

## <a name="111170"></a>1.1.117.0
发布时间：2016 年 3 月

**新连接器**  
[泛型 SQL 连接器](active-directory-aadconnectsync-connector-genericsql.md)的初始版本。

**新功能：**

- 通用 LDAP 连接器：
  - 添加了对通过 Isode 进行增量导入的支持。
- Web 服务连接器：
  - 更新了 csEntryChangeResult 活动和 setImportErrorCode 活动，以允许将对象级别错误返回到同步引擎。
  - 更新了 SAP6 和 SAP6User 模板，以使用新的对象级别错误功能。
- Lotus Domino 连接器：
  - 导出时，每个通讯簿需要一个认证者。 现在可以对所有认证者使用同一密码以简化管理。

**已解决的问题：**

- 通用 LDAP 连接器：
  - 对于 IBM Tivoli DS，未正确检测到某些引用属性。
  - 对于增量导入过程中的 Open LDAP，截去了字符串开头和结尾的空格。
  - 对于 Novell 和 NetIQ，导出（在 OU/容器之间移动了对象，同时重命名了对象）失败。
- Web 服务连接器：
  - 如果 Web 服务对于同一绑定具有多个终结点，则连接器未正确发现这些终结点。
- Lotus Domino 连接器：
  - 将 fullName 属性导出到邮件数据库不正常工作。
  - 同时从组中添加和删除成员的导出仅导出了所添加的成员。
  - 如果 Notes Document 无效（isValid 属性设置为 false），则连接器将失败。

## <a name="older-releases"></a>较旧版本
在 2016 年 3 月之前，连接器已发布为支持主题。

**通用 LDAP**

- [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597，2015 年 9 月
- [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549，2015 年 3 月
- [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534，2015 年 1 月
- [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419，2014 年 9 月
- [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082，2014 年 3 月

**WebServices**

- [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419，2014 年 9 月

**PowerShell**

- [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419，2014 年 9 月

**Lotus Domino**

- [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597，2015 年 9 月
- [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549，2015 年 3 月
- [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712，2014 年 8 月
- [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003，2014 年 2 月  
- [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721，2013 年 10 月
- [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534，2013 年 8 月

## <a name="next-steps"></a>后续步骤
了解有关 [Azure AD Connect 同步](active-directory-aadconnectsync-whatis.md)配置的详细信息。

了解有关[将本地标识与 Azure Active Directory 集成](active-directory-aadconnect.md)的详细信息。

<!--Update_Description: wording update -->