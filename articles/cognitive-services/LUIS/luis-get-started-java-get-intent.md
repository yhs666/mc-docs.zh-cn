---
title: 获取意向，Java
titleSuffix: Language Understanding - Azure Cognitive Services
description: 本 Java 快速入门使用可用的公共 LUIS 应用从会话文本中确定用户的意向。
author: lingliw
manager: digimobile
services: cognitive-services
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 04/19/19
ms.author: v-lingwu
ms.openlocfilehash: 007aec262da1a675296bab70cfbdbb49b31be827
ms.sourcegitcommit: e77582e79df32272e64c6765fdb3613241671c20
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2019
ms.locfileid: "67135923"
---
# <a name="quickstart-get-intent-using-java"></a>快速入门：使用 Java 获取意向

在本快速入门中，你将向 LUIS 终结点传递话语并返回意向和实体。

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>先决条件

* [JDK SE](https://docs.azure.cn/zh-cn/java/java-supported-jdk-runtime?view=azure-java-stable)（Java 开发工具包，标准版）
* [Visual Studio Code](https://code.visualstudio.com/) 或你喜欢用的 IDE
* 公共应用 ID：df67dcdb-c37d-46af-88e1-8b97951ca1c2

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>获取 LUIS 密钥

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>使用浏览器获取意向

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>以编程方式获取意向

可以使用 Java 来访问上一步骤中浏览器窗口显示的相同结果。 请务必将 Apache 库添加到项目。

1. 复制以下代码以在名为 `LuisGetRequest.java` 的文件中创建一个类：

```
    // This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/downloads.cgi)

    // You need to add the following Apache HTTP client libraries to your project:
    // httpclient-4.5.3.jar
    // httpcore-4.4.11.jar
    // commons-logging-4.0.6.jar

    import java.net.URI;
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.HttpClient;
    import org.apache.http.client.methods.HttpGet;
    import org.apache.http.client.utils.URIBuilder;
    import org.apache.http.impl.client.HttpClients;
    import org.apache.http.util.EntityUtils;

    public class LuisGetRequest {

        public static void main(String[] args) 
        {
            HttpClient httpclient = HttpClients.createDefault();

            try
            {

                // The ID of a public sample LUIS app that recognizes intents for turning on and off lights
                String AppId = "df67dcdb-c37d-46af-88e1-8b97951ca1c2";
                
                // Add your endpoint key 
                // You can use the authoring key instead of the endpoint key. 
                // The authoring key allows 1000 endpoint queries a month.
                String EndpointKey = "YOUR-KEY";

                // Begin endpoint URL string building
                URIBuilder endpointURLbuilder = new URIBuilder("https://chinaeast2.api.cognitive.azure.cn/luis/v2.0//apps/" + AppId + "?");

                // query text
                endpointURLbuilder.setParameter("q", "turn on the left light");

                // create URL from string
                URI endpointURL = endpointURLbuilder.build();

                // create HTTP object from URL
                HttpGet request = new HttpGet(endpointURL);

                // set key to access LUIS endpoint
                request.setHeader("Ocp-Apim-Subscription-Key", EndpointKey);

                // access LUIS endpoint - analyze text
                HttpResponse response = httpclient.execute(request);

                // get response
                HttpEntity entity = response.getEntity();


                if (entity != null) 
                {
                    System.out.println(EntityUtils.toString(entity));
                }
            }

            catch (Exception e)
            {
                System.out.println(e.getMessage());
            }
        }
    }
```

2. 将 `YOUR-KEY` 变量的值替换为自己的 LUIS 密钥。

3. 替换为自己的文件路径并从命令行编译 java 程序：`javac -cp .;<FILE_PATH>\* LuisGetRequest.java`。

4. 替换为自己的文件路径并从命令行运行应用程序：`java -cp .;<FILE_PATH>\* LuisGetRequest.java`。 其中会显示前面在浏览器窗口中显示的相同 JSON。

    ![控制台窗口显示 LUIS 中的 JSON 结果](./media/luis-get-started-java-get-intent/console-turn-on.png)
    
## <a name="luis-keys"></a>LUIS 密钥

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>清理资源

删除 Java 文件/项目文件夹。

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [添加表达](luis-get-started-java-add-utterance.md)




