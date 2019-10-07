---
title: Azure 流分析预览功能
description: 本文列出了当前以预览版提供的 Azure 流分析功能。
services: stream-analytics
author: lingliw
ms.author: v-lingwu
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
origin.date: 05/29/2019
ms.date: 08/09/2019
ms.openlocfilehash: e461ed8bdb20761e3fa7f3079ec55ecd8f829e64
ms.sourcegitcommit: c72fba1cacef1444eb12e828161ad103da338bb1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71674827"
---
# <a name="azure-stream-analytics-preview-features"></a>Azure 流分析预览功能

本文汇总了当前以预览版提供的 Azure 流分析的所有功能。 建议不要在生产环境中使用预览功能。

## <a name="public-previews"></a>公共预览版

以下功能以公共预览版提供。 现在可以使用这些功能，但请勿在生产环境中使用它们。

### <a name="one-click-integration-with-event-hubs"></a>与事件中心一键集成 
通过此集成，现在可以直观地显示传入数据，并从事件中心门户单击一下即可开始编写流分析查询。 查询准备就绪后，只需单击几下鼠标就能将其产品化并开始获得实时见解。 这可以显著减少开发实时分析解决方案所需的时间和成本。 [此处](/event-hubs/process-data-azure-stream-analytics)可获取文档。

### <a name="visual-studio-code-for-azure-stream-analytics"></a>适用于 Azure 流分析的 Visual Studio Code

可以在 Visual Studio Code 中创建 Azure 流分析作业。 请参阅我们的 [VS Code 入门教程](/stream-analytics/quick-create-vs-code)。

### <a name="anomaly-detection"></a>异常检测

Azure 流分析引入了新的机器学习模型，除了支持双向、慢正和慢负趋势检测外，还支持“峰值”和“低值”检测   。 

### <a name="integration-with-azure-machine-learning"></a>与 Azure 机器学习集成

可使用机器学习 (ML) 函数缩放流分析作业。 

### <a name="javascript-user-defined-aggregate"></a>JavaScript 用户定义的聚合

Azure 流分析支持以 JavaScript 编写的用户定义的聚合 (UDA)，可实现复杂的有状态业务逻辑。 参阅 [Azure 流分析 JavaScript 用户定义的聚合](stream-analytics-javascript-user-defined-aggregates.md)文档，了解如何使用 UDA。 

### <a name="live-data-testing-in-visual-studio"></a>Visual Studio 中的实时数据测试

适用于 Azure 流分析的 Visual Studio 工具增强了本地测试功能，通过该功能可针对来自云源（如事件中心或 IoT 中心）的实时事件流测试查询。 了解如何[使用适用于 Visual Studio 的 Azure 流分析工具在本地测试实时数据](stream-analytics-live-data-local-testing.md)。

### <a name="net-user-defined-functions-on-iot-edge"></a>IoT Edge 上的 .NET 用户定义函数

使用 .NET Standard 用户定义函数，可以将 .NET Standard 代码作为流式管道的一部分运行。 可以创建简单的 C# 类或导入完整的项目和库。 Visual Studio 支持完整的创作和调试体验。 有关详细信息，请访问[为 Azure 流分析 Edge 作业开发 .NET Standard 用户定义函数](stream-analytics-edge-csharp-udf-methods.md)。

## <a name="other-previews"></a>其他预览版功能

以下功能也可根据要求在预览版中使用。

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge-and-cloud"></a>Azure IoT Edge 流分析和云的 C# 自定义反序列化程序

开发人员可以在 C# 中实现自定义反序列化程序，对 Azure 流分析接收的事件进行反序列化。 可以进行反序列化的格式示例包括 Parquet、Protobuf、XML 或任何二进制格式。 在[此处](https://aka.ms/asapreview1)注册此预览版。

### <a name="support-for-azure-stack"></a>支持 Azure Stack
此功能在 Azure IoT Edge 运行时上启用，可利用自定义 Azure Stack 功能，例如对在 Azure Stack 上运行的本地输入和输出的本机支持（例如，事件中心、IoT 中心、Blob 存储）。 利用这一新的集成，可以构建混合体系结构，可以在数据生成位置附近分析数据，从而降低延迟并最大限度地提高见解。
在[此处](https://aka.ms/asapreview1)注册此预览版。

