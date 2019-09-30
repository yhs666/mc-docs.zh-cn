---
title: 快速入门：更改、训练应用，C# - LUIS
titleSuffix: Azure Cognitive Services
description: 此 C# 快速入门将示例话语添加到家庭自动化应用并训练该应用。
services: cognitive-services
author: lingliw
manager: digimobile
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
origin.date: 09/04/2019
ms.date: 09/23/2019
ms.author: v-lingwu
ms.openlocfilehash: 34438bc9852dfb9fd332da08c2c58d141c899e55
ms.sourcegitcommit: 2f2ced6cfaca64989ad6114a6b5bc76700870c1a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71330417"
---
# <a name="quickstart-change-model-using-c"></a>快速入门：使用 C# 更改模型

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-change-model-intro-para.md)]

## <a name="prerequisites"></a>先决条件

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* 最新的 [**Visual Studio Community Edition**](https://www.visualstudio.com/downloads/)。
* C# 编程语言已安装。
* [JsonFormatterPlus](https://www.nuget.org/packages/JsonFormatterPlus) 和 [CommandLine](https://www.nuget.org/packages/CommandLineParser/) NuGet 包

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>示例话语 JSON 文件

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>创建快速入门代码 

在 Visual Studio 中，使用 .NET Framework 创建新的 **Windows 经典桌面控制台**应用。 将项目命名为 `ConsoleApp1`。

![Visual Studio 项目类型](./media/luis-quickstart-cs-add-utterance/vs-project-type.png)

### <a name="add-the-systemweb-dependency"></a>添加 System.Web 依赖项

Visual Studio 项目需要 **System.Web**。 在解决方案资源管理器中，右键单击“引用”并从“程序集”部分中选择“添加引用”。  

![添加 System.web 引用](./media/luis-quickstart-cs-add-utterance/system.web.png)

### <a name="add-other-dependencies"></a>添加其他依赖项

Visual Studio 项目需要 **JsonFormatterPlus** 和 **CommandLineParser**。 在“解决方案资源管理器”中，右键单击“引用”，并选择“管理 NuGet 包...”   。浏览并添加这两个包中的每一个包。 

![添加第三方依赖项](./media/luis-quickstart-cs-add-utterance/add-dependencies.png)


### <a name="write-the-c-code"></a>编写 C# 代码
**Program.cs** 文件应为：

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

更新依赖项，以便：

```
      using System;
      using System.IO;
      using System.Net.Http;
      using System.Text;
      using System.Threading.Tasks;
      using System.Collections.Generic;
      using System.Linq;

      // 3rd party NuGet packages
      using JsonFormatterPlus;
      using CommandLine;
```


将 LUIS ID 和字符串添加到 **Program** 类。
```
      // NOTE: Replace this example LUIS application ID with the ID of your LUIS application.
      static string appID = "YOUR-APP-ID";

      // NOTE: Replace this example LUIS application version number with the version number of your LUIS application.
      static string appVersion = "0.1";

      // NOTE: Replace this example LUIS authoring key with a valid key.
      static string authoringKey = "YOUR-AUTHORING-KEY";

      // Uses chinaeast region
      static string host = "https://chinaeast2.api.cognitive.azure.cn";
      static string path = "/luis/api/v2.0/apps/" + appID + "/versions/" + appVersion + "/";
```

向 **Program** 类添加用于管理命令行参数的类。

```
      // parse command line options 
      public class Options
      {
         [Option('v', "verbose", Required = false, HelpText = "Set output to verbose messages.")]
         public bool Verbose { get; set; }

         [Option('t', "train", Required = false, HelpText = "Train model.")]
         public bool Train { get; set; }

         [Option('s', "status", Required = false, HelpText = "Get training status.")]
         public bool Status { get; set; }

         [Option('a', "add", Required = false, HelpText = "Add example utterances to model.")]
         public IEnumerable<string> Add{ get; set; }
      }
```

将 GET 请求方法添加到 **Program** 类。

```
      async static Task<HttpResponseMessage> SendGet(string uri)
      {
         using (var client = new HttpClient())
         using (var request = new HttpRequestMessage())
         {
            request.Method = HttpMethod.Get;
            request.RequestUri = new Uri(uri);
            request.Headers.Add("Ocp-Apim-Subscription-Key", authoringKey);
            return await client.SendAsync(request);
         }
      }
```


将 POST 请求方法添加到 **Program** 类。 

```
      async static Task<HttpResponseMessage> SendPost(string uri, string requestBody)
      {
         using (var client = new HttpClient())
         using (var request = new HttpRequestMessage())
         {
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(uri);

            if (!String.IsNullOrEmpty(requestBody))
            {
                  request.Content = new StringContent(requestBody, Encoding.UTF8, "text/json");
            }

            request.Headers.Add("Ocp-Apim-Subscription-Key", authoringKey);
            return await client.SendAsync(request);
         }
      }
```

将示例话语从文件方法添加到 **Program** 类。

```
      async static Task AddUtterances(string input_file)
      {
         string uri = host + path + "examples";
         string requestBody = File.ReadAllText(input_file);

         var response = await SendPost(uri, requestBody);
         var result = await response.Content.ReadAsStringAsync();
         Console.WriteLine("Added utterances.");
         Console.WriteLine(JsonFormatter.Format(result));
      }
```

将更改应用到模型以后，训练该模型。 将方法添加到 **Program** 类。

```
      async static Task Train()
      {
         string uri = host + path + "train";

         var response = await SendPost(uri, null);
         var result = await response.Content.ReadAsStringAsync();
         Console.WriteLine("Sent training request.");
         Console.WriteLine(JsonFormatter.Format(result));

      }
```

训练可能不会立刻完成，请通过查看状态来验证训练是否已完成。 将方法添加到 **Program** 类。

```
      async static Task Status()
      {
         var response = await SendGet(host + path + "train");
         var result = await response.Content.ReadAsStringAsync();
         Console.WriteLine("Requested training status.");
         Console.WriteLine(JsonFormatter.Format(result));
      }
```

若要管理命令行参数，请添加 main 代码。 将方法添加到 **Program** 类。

```
      static void Main(string[] args)
            {

                  // Parse commandline options
                  // For example: 
                  // ConsoleApp1.exe --add utterances.json --train --status
                  Parser.Default.ParseArguments<Options>(args)
                                    .WithParsed<Options>(o =>
                                    {
                                       
                                       // add example utterances
                                       if (o.Add != null && o.Add.GetEnumerator().MoveNext())
                                       {
                                             AddUtterances(o.Add.FirstOrDefault()).Wait();
                                       }

                                       // request training
                                       if (o.Train)
                                       {
                                             Train().Wait();

                                       }

                                       // get training status
                                       if (o.Status)
                                       {
                                             Status().Wait();

                                       }
                                    });

            }
         }
      }
```

### <a name="copy-utterancesjson-to-output-directory"></a>将 utterances.json 复制到输出目录

在解决方案资源管理器中，通过右键单击解决方案资源管理器的项目名称，然后依次选择“添加”  、“现有项”  ，添加 `utterances.json`。 选择 `utterances.json` 文件。 这会将文件添加到项目。 然后需要将它添加到输出目录。 右键单击 `utterances.json` 并选择“属性”  。 在属性窗口中，标记 `Content` 的“生成操作”，并标记 `Copy Always` 的“复制到输出目录”。    

![将 JSON 文件标记为内容](./media/luis-quickstart-cs-add-utterance/content-properties.png)

## <a name="build-code"></a>生成代码

在 Visual Studio 中生成代码。 

## <a name="run-code"></a>运行代码

在项目的 /bin/Debug 目录中，从命令行运行应用程序。 

```console
ConsoleApp1.exe --add utterances.json --train --status
```

此命令行显示调用“添加陈述”API 的结果。 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]

## <a name="clean-up-resources"></a>清理资源
完成本快速入门后，删除在本快速入门中创建的所有文件。 

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"] 
> [以编程方式生成 LUIS 应用](luis-tutorial-node-import-utterances-csv.md) 




