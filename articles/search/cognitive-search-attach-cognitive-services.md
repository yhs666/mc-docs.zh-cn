---
title: 将认知服务资源与技能集联系起来 - Azure 搜索
description: 说明如何将认知服务一体化订阅附加到 Azure 搜索中的认知扩充管道。
manager: nitinme
author: LuisCabrer
services: search
ms.service: search
ms.subservice: cognitive-search
ms.topic: conceptual
origin.date: 05/20/2019
ms.date: 09/25/2019
ms.author: v-tawe
ms.openlocfilehash: 3b4716ad2ec69345695559015f56eabebd276a34
ms.sourcegitcommit: a5a43ed8b9ab870f30b94ab613663af5f24ae6e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71674414"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>将认知服务资源与 Azure 搜索中的技能集联系起来 

AI 算法驱动用于 Azure 搜索中文档扩充的[认知索引管道](cognitive-search-concept-intro.md)。 这些算法基于 Azure 认知服务资源，包括[计算机视觉](https://www.azure.cn/home/features/cognitive-services/computer-vision/)（用于图像分析和光学字符识别 (OCR)），以及[文本分析](https://www.azure.cn/home/features/cognitive-services/text-analytics/)（用于实体识别、关键短语提取和其他扩充操作）。 由于 Azure 搜索用于文档扩充，因此算法包装在“技能”  中，放置在“技能组”  中，并在索引过程中由索引器  引用。

可以免费扩充有限数量的文档。 或者，可以将可计费的认知服务资源附加到*技能组*上，以处理更大和更频繁的工作负荷。 本文介绍如何在 Azure 搜索[索引](search-what-is-an-index.md)期间附加可计费认知服务资源以扩充文档。

> [!NOTE]
> 在 Azure 搜索文档破解阶段中，可计费事件包括对认知服务 API 的调用和图像提取。 对于从文档中提取文本或不调用认知服务的技能，不收取任何费用。
>
> 按[认知服务预付费价格](https://www.azure.cn/pricing/details/cognitive-services/)执行可计费技能。 有关图像提取定价，请参阅 [Azure 搜索定价页](https://www.azure.cn/pricing/details/search/)。

## <a name="same-region-requirement"></a>相同区域要求

我们要求 Azure 搜索和 Azure 认知服务位于同一区域。 否则，将在运行时收到此消息：`"Provided key is not a valid CognitiveServices type key for the region of your search service."` 

无法跨区域移动服务。 如果出现此错误，应该在 Azure 搜索所在的区域中创建一个新的认知服务资源。

> [!NOTE]
> 某些内置技能基于非区域认知服务（例如，[文本翻译技能](cognitive-search-skill-text-translation.md)）。 请注意，如果你将这些技能中的任何一项添加到技能组，则数据不能保证与 Azure 搜索或认知服务资源位于同一区域。 有关更多详细信息，请参阅[服务状态页](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=china-non-regional,china-east,china-east-2,china-north,china-north-2)。

## <a name="use-free-resources"></a>使用免费资源

可以使用有限的免费处理选项来完成认知搜索教程和快速入门练习。

免费（有限扩充）资源限制为每个订阅每天 20 个文档。

1. 打开导入数据向导：

   ![打开导入数据向导](media/search-get-started-portal/import-data-cmd.png "打开导入数据向导")

1. 选择数据源，然后转到“添加认知搜索(可选)”。  有关此向导的分步演练，请参阅[使用门户工具进行导入、索引编制和查询](search-get-started-portal.md)。

1. 展开“附加认知服务”，然后选择“免费(有限扩充)”：  

   ![展开的“附加认知服务”部分](./media/cognitive-search-attach-cognitive-services/attach1.png "展开的“附加认知服务”部分")

1. 转到下一步骤“添加扩充”。  有关门户中提供的技能的说明，请参阅认知搜索快速入门中的[步骤 2：添加认知技能](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline)。

## <a name="use-billable-resources"></a>使用付费资源

对于每天创建超过 20 个扩充的工作负荷，请确保附加可计费的认知服务资源。 我们建议你始终附加可计费的认知服务资源，即使你从未打算调用认知服务 API 也是如此。 附加资源会重写每日限制。

只有调用认知服务 API 的技能才收费。 你无需为[文本合并器](cognitive-search-skill-textmerger.md)、[文本拆分器](cognitive-search-skill-textsplit.md)和[整形程序](cognitive-search-skill-shaper.md)付费，这些都不基于 API。

<!--  [custom skills](cognitive-search-create-custom-skill-example.md), or skills like  -->

1. 打开导入数据向导，选择数据源，然后转到“添加认知搜索(可选)”。 

1. 展开“附加认知服务”，然后选择“创建新的认知服务资源”。   此时会打开一个新的选项卡让你创建资源：

   ![创建认知服务资源](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "创建认知服务资源")

1. 在“位置”列表中，选择你的 Azure 搜索服务所在的区域。  出于性能方面的原因，请确保使用此区域。 使用此区域还可避免跨区域的出站带宽费用。

1. 在“定价层”列表中，选择“S0”获取认知服务功能一体化集合，包括为 Azure 搜索使用的预定义技能提供支持的“视觉和语言”功能。  

   对于 S0 层，可以在[认知服务定价页](https://www.azure.cn/pricing/details/cognitive-services/)上找到特定工作负荷的费率。
  
   + 在“选择套餐”列表中，确保“认知服务”已选中。  
   + 在“语言”功能下，“文本分析标准版”的费率适用于 AI 索引。  
   + 在“视觉”功能下，适用“计算机视觉 S1”的费率。  

1. 选择“创建”预配新的认知服务资源。 

1. 返回到包含导入数据向导的上一选项卡。 选择“刷新”显示该认知服务资源，然后选择该资源。 

   ![选择认知服务资源](./media/cognitive-search-attach-cognitive-services/attach2.png "选择认知服务资源")

1. 展开“添加扩充”部分，选择要针对数据运行的特定认知技能。  完成向导中的剩余步骤。 有关门户中提供的技能的说明，请参阅认知搜索快速入门中的[步骤 2：添加认知技能](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline)。

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>将现有技能集附加到认知服务资源

如果你有现有的技能集，可将其附加到新的或不同的认知服务资源。

1. 在“服务概述”页上选择“技能集”：  

   ![“技能集”选项卡](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "“技能集”选项卡")

1. 选择技能集的名称，然后选择现有资源或新建资源。 选择“确定”以确认所做的更改。 

   ![技能集资源列表](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "技能集资源列表")

   请记住，“免费(有限扩充)”选项限制为每日 20 个文档，可以使用“创建新的认知服务资源”预配新的可计费资源。   如果创建了新资源，请选择“刷新”以刷新认知服务资源的列表，然后选择该资源。 

## <a name="attach-cognitive-services-programmatically"></a>以编程方式附加认知服务

以编程方式定义技能集时，请将 `cognitiveServices` 节添加到该技能集。 在该节中，包含要与该技能集关联的认知服务资源的键。 请记住，该资源必须位于 Azure 搜索资源所在的同一区域。 另外请包含 `@odata.type`，并将其设置为 `#Microsoft.Azure.Search.CognitiveServicesByKey`。

以下示例演示了此模式。 请注意定义末尾的 `cognitiveServices` 节。

```http
PUT https://[servicename].search.chinacloudapi.cn/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>示例：估算成本

若要估算认知搜索索引编制的相关成本，请先构想一下文档的大致结构，以便能够得出一些数字。 例如，可以使用以下似近值：

+ 1,000 个 PDF。
+ 每个 PDF 有 6 个页面。
+ 每个页面包含一个图像（共 6,000 个图像）。
+ 每个页面包含 3,000 个字符。

假设管道的功能包括：每个 PDF 的文档破解、图像和文本提取、图像的光学字符识别 (OCR)，以及组织的实体识别。

本文中所示的价格是虚构的。 这些价格用于演示估算过程。 你的成本可能更低。 有关实际交易价格，请参阅[认知服务定价](https://www.azure.cn/pricing/details/cognitive-services/)。

1. 破解包含文本和图像内容的文档时，文本提取目前是免费的。 对于 6,000 个图像，假设每提取 1,000 个图像需要 $1。 则此步骤的成本是 $6.00。

2. 对于英语环境中 6000 个图像的 OCR，OCR 认知技能使用最佳算法 (DescribeText)。 假设每分析 1,000 个图像的成本为 $2.50，则这一步需要支付 $15.00。

3. 提取实体时，每页总共有 3 个文本记录。 每条记录包含 1,000 个字符。 每页 3 个文本记录 * 6,000 页 = 18,000 个文本记录。 假设 1000 个文本记录的成本为 $2.00，则这一步的成本为 $36.00。

综合起来，在使用上述技能集引入 1,000 个此类 PDF 文档时，需要支付大约 $57.00。

## <a name="next-steps"></a>后续步骤
+ [Azure 搜索定价页](https://www.azure.cn/pricing/details/search/)
+ [如何定义技能集](cognitive-search-defining-skillset.md)
+ [创建技能组合 (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [如何映射扩充的域](cognitive-search-output-field-mapping.md)
