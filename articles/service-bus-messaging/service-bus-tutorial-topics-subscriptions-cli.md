---
title: 教程 - 通过 Azure CLI 使用发布/订阅渠道和主题筛选器更新零售库存分类 | Azure Docs
description: 在本教程中，你将了解如何从主题和订阅发送和接收消息，以及如何使用 Azure CLI 添加和使用筛选器规则
services: service-bus-messaging
author: lingliw
manager: digimobile
ms.author: v-lingwu
ms.date: 01/21/19
ms.topic: tutorial
ms.service: service-bus-messaging
ms.custom: mvc
ms.openlocfilehash: 0c8e0a3abf660045c15bd796dc58ae2d04016f0a
ms.sourcegitcommit: 26957f1f0cd708f4c9e6f18890861c44eb3f8adf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2019
ms.locfileid: "54363517"
---
# <a name="tutorial-update-inventory-using-cli-and-topicssubscriptions"></a>教程：使用 CLI 和主题/订阅更新库存

世纪互联 Azure 服务总线是一种多租户云消息传送服务，可以在应用程序和服务之间发送信息。 异步操作可实现灵活的中转消息传送、结构化的先进先出 (FIFO) 消息传送以及发布/订阅功能。 本教程展示了如何使用 Azure CLI 和 Java 在零售库存方案中将服务总线主题和订阅与发布/订阅频道配合使用。

本教程介绍如何执行下列操作：
> [!div class="checklist"]
> * 使用 Azure CLI 创建一个服务总线主题和一个或多个对该主题的订阅
> * 使用 Azure CLI 添加主题筛选器
> * 创建两条具有不同内容的消息
> * 发送消息并验证它们是否已到达预期的订阅
> * 从订阅接收消息

此方案的一个示例是为多个零售店更新库存分类。 在此方案中，每个商店或商店组都获取适用于它们的消息来更新其分类。 本教程展示了如何使用订阅和筛选器实现此方案。 首先，创建一个包含 3 个订阅的主题，添加一些规则和筛选器，然后从主题和订阅发送和接收消息。

![主题](./media/service-bus-tutorial-topics-subscriptions-cli/about-service-bus-topic.png)

如果没有 Azure 订阅，可在开始前创建一个[试用帐户][]。

## <a name="prerequisites"></a>先决条件

若要使用 Java 开发服务总线应用，必须安装以下项：

- [Java 开发工具包](https://aka.ms/azure-jdks)最新版本。
- [Azure CLI](https://docs.azure.cn/zh-cn/cli/index?view=azure-cli-latest)
- [Apache Maven](https://maven.apache.org) 3.0 或更高版本。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

如果选择在本地安装并使用 CLI，本教程要求运行 Azure CLI 2.0.4 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="service-bus-topics-and-subscriptions"></a>服务总线主题和订阅

每个[对主题的订阅](service-bus-messaging-overview.md#topics)都可以接收每条消息的副本。 主题在协议和语义方面与服务总线队列完全兼容。 服务总线主题支持一系列选择规则，这些规则具有筛选条件和用来设置或修改消息属性的可选操作。 规则每次匹配时，都会生成一条消息。 若要深入了解规则、筛选器和操作，请单击此[链接](topic-filters.md)。

## <a name="sign-in-to-azure"></a>登录 Azure

安装 CLI 后，打开一个命令提示符并发出以下命令来登录到 Azure。 如果使用的是 Cloud Shell，则这些步骤不是必需的：

1. 如果在本地使用 Azure CLI，请运行以下命令来登录到 Azure。 如果在 Cloud Shell 中运行这些命令，则此登录步骤不是必需的：

   ```Azure CLI az login
   ```

2. Set the current subscription context to the Azure subscription you want to use:

   ```Azure CLI
   az account set --subscription Azure_subscription_name
   ```

## <a name="use-cli-to-provision-resources"></a>使用 CLI 来预配资源

发出以下命令来预配服务总线资源。 请务必将所有占位符替换为适当的值：

```Azure CLI
# <a name="create-a-resource-group"></a>创建资源组
az group create --name myResourcegroup --location chinaeast

# <a name="create-a-service-bus-messaging-namespace-with-a-unique-name"></a>创建具有唯一名称的服务总线消息传递命名空间
namespaceName=myNameSpace$RANDOM az servicebus namespace create \
   --resource-group myResourceGroup \
   --name $namespaceName \
   --location chinaeast

# <a name="create-a-service-bus-topic"></a>创建服务总线主题
az servicebus topic create --resource-group myResourceGroup \
   --namespace-name $namespaceName \
   --name myTopic

# <a name="create-subscription-1-to-the-topic"></a>创建主题的订阅 1
az servicebus subscription create --resource-group myResourceGroup --namespace-name $namespaceName --topic-name myTopic --name S1

# <a name="create-filter-1---use-custom-properties"></a>创建筛选器 1 - 使用自定义属性
az servicebus rule create --resource-group myResourceGroup --namespace-name $namespaceName --topic-name myTopic --subscription-name S1 --name MyFilter --filter-sql-expression "StoreId IN ('Store1','Store2','Store3')"

# <a name="create-filter-2---use-custom-properties"></a>创建筛选器 2 - 使用自定义属性
az servicebus rule create --resource-group myResourceGroup --namespace-name $namespaceName --topic-name myTopic --subscription-name S1 --name MySecondFilter --filter-sql-expression "StoreId = 'Store4'"

# <a name="create-subscription-2"></a>创建订阅 2
az servicebus subscription create --resource-group myResourceGroup --namespace-name $namespaceName --topic-name myTopic --name S2

# <a name="create-filter-3---use-message-header-properties-via-in-list-and"></a>创建筛选器 3 - 通过 IN 列表使用消息标头属性并 
# <a name="combine-with-custom-properties"></a>将其与自定义属性结合使用。
az servicebus rule create --resource-group myResourceGroup --namespace-name $namespaceName --topic-name myTopic --subscription-name S2 --name MyFilter --filter-sql-expression "sys.To IN ('Store5','Store6','Store7') OR StoreId = 'Store8'"

# <a name="create-subscription-3"></a>创建订阅 3
az servicebus subscription create --resource-group myResourceGroup --namespace-name $namespaceName --topic-name myTopic --name S3

# <a name="create-filter-4---get-everything-except-messages-for-subscription-1-and-2"></a>创建筛选器 4 - 获取订阅 1 和 2 的除消息外的所有内容。 
# <a name="also-modify-and-add-an-action-in-this-case-set-the-label-to-a-specified-value"></a>另请修改和添加操作；在此示例中，请将标签设置为指定的值。 
# <a name="assume-those-stores-might-not-be-part-of-your-main-store-so-you-only-add"></a>假定这些存储可能不是主存储的一部分，因此只向其添加 
# <a name="specific-items-to-them-for-that-you-flag-them-specifically"></a>特定项目。 为此，请对其进行专门的标记。
az servicebus rule create --resource-group DemoGroup --namespace-name DemoNamespaceSB --topic-name tutorialtest1 --subscription-name S3 --name MyFilter --filter-sql-expression "sys.To NOT IN ('Store1','Store2','Store3','Store4','Sto re5','Store6','Store7','Store8') OR StoreId NOT IN ('Store1','Store2','Store3','Store4','Store5','Store6','Store7','Stor e8')" --action-sql-expression "SET sys.Label = 'SalesEvent'"

# <a name="get-the-connection-string"></a>获取连接字符串
connectionString=$(az servicebus namespace authorization-rule keys list \
   --resource-group myResourceGroup \
   --namespace-name  $namespaceName \
   --name RootManageSharedAccessKey \
   --query primaryConnectionString --output tsv)
```

After the last command runs, copy and paste the connection string, and the queue name you selected, to a temporary location such as Notepad. You will need it in the next step.

## Create filter rules on subscriptions

After the namespace and topic/subscriptions are provisioned, and you have the necessary credentials, you are ready to create filter rules on the subscriptions, then send and receive messages. You can examine the code in [this GitHub sample folder](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/azure-servicebus/TopicFilters).

## Send and receive messages

1. Make sure that Cloud Shell is open and displaying the Bash prompt.

2. Clone the [Service Bus GitHub repository](https://github.com/Azure/azure-service-bus/) by issuing the following command:

   ```shell
   git clone https://github.com/Azure/azure-service-bus.git
   ```

2. 导航到示例文件夹 `azure-service-bus/samples/Java/quickstarts-and-tutorials/quickstart-java/tutorial-topics-subscriptions-filters-java`。 请注意，在 Bash shell 中，命令区分大小写，并且路径分隔符必须为正斜杠。

3. 发出以下命令来生成应用程序：
   
   ```shell
   mvn clean package -DskipTests
   ```
4. 若要运行程序，请发出以下命令。 请确保将占位符替换为在上一步骤中获取的连接字符串和主题名称：

   ```shell
  java -jar .\target\tutorial-topics-subscriptions-filters-1.0.0-jar-with-dependencies.jar -c "myConnectionString" -t "myTopicName"
   ```

   观察发送到主题并随后从各个订阅中接收的 10 条消息：

   ![程序输出](./media/service-bus-tutorial-topics-subscriptions-cli/service-bus-tutorial-topics-subscriptions-cli.png)

## <a name="clean-up-resources"></a>清理资源

运行以下命令来删除资源组、命名空间和所有相关资源：

```Azure CLI az group delete --resource-group my-resourcegroup
```

## Understand the sample code

This section contains more details about what the sample code does.

### Get connection string and queue

First, the code declares a set of variables, which drive the remaining execution of the program:

```java
    public String ConnectionString = null;
    public String TopicName = null;
    static final String[] Subscriptions = {"S1","S2","S3"};
    static final String[] Store = {"Store1","Store2","Store3","Store4","Store5","Store6","Store7","Store8","Store9","Store10"};
    static final String SysField = "sys.To";
    static final String CustomField = "StoreId";    
    int NrOfMessagesPerStore = 1; // Send at least 1.
```

连接字符串和主题名称是通过命令行参数添加并传递到 `main()` 的唯一值。 实际代码执行是在 `run()` 方法中触发的并且会从主题接收消息：

```java
public static void main(String[] args) {
        TutorialTopicsSubscriptionsFilters app = new TutorialTopicsSubscriptionsFilters();
        try {
            app.runApp(args);
            app.run();
        } catch (Exception e) {
            System.out.printf("%s", e.toString());
        }
        System.exit(0);
    }
}

public void run() throws Exception {
        // Send sample messages.
        this.sendMessagesToTopic();

        // Receive messages from subscriptions.
        this.receiveAllMessages();
}
```

### <a name="create-topic-client-to-send-messages"></a>创建主题客户端来发送消息

为发送和接收消息，`sendMessagesToTopic()` 方法创建一个主题客户端实例（该实例使用连接字符串和主题名称），然后调用发送消息的另一个方法：

```java
public void sendMessagesToTopic() throws Exception, ServiceBusException {
         // Create client for the topic.
        TopicClient topicClient = new TopicClient(new ConnectionStringBuilder(ConnectionString, TopicName));

        // Create a message sender from the topic client.

        System.out.printf("\nSending orders to topic.\n");

        // Now we can start sending orders.
        CompletableFuture.allOf(
                SendOrders(topicClient,Store[0]),
                SendOrders(topicClient,Store[1]),
                SendOrders(topicClient,Store[2]),
                SendOrders(topicClient,Store[3]),
                SendOrders(topicClient,Store[4]),
                SendOrders(topicClient,Store[5]),
                SendOrders(topicClient,Store[6]),
                SendOrders(topicClient,Store[7]),
                SendOrders(topicClient,Store[8]),
                SendOrders(topicClient,Store[9])                
        ).join();

        System.out.printf("\nAll messages sent.\n");
    }

     public CompletableFuture<Void> SendOrders(TopicClient topicClient, String store) throws Exception {

        for(int i = 0;i<NrOfMessagesPerStore;i++) {
            Random r = new Random();
            final Item item = new Item(r.nextInt(5),r.nextInt(5),r.nextInt(5));         
            IMessage message = new Message(GSON.toJson(item,Item.class).getBytes(UTF_8)); 
            // We always set the Sent to field
            message.setTo(store);    
            final String StoreId = store;
            Double priceToString = item.getPrice();
            final String priceForPut = priceToString.toString();
            message.setProperties(new HashMap<String, String>() {{
                // Additionally we add a customer store field. In reality you would use sys.To or a customer property but not both. 
                // This is just for demo purposes.
                put("StoreId", StoreId);
                // Adding more potential filter / rule and action able fields
                put("Price", priceForPut);
                put("Color", item.getColor());
                put("Category", item.getItemCategory());
            }});
                        
            System.out.printf("Sent order to Store %s. Price=%f, Color=%s, Category=%s\n", StoreId, item.getPrice(), item.getColor(), item.getItemCategory());            
            topicClient.sendAsync(message);
        }
               
        return new CompletableFuture().completedFuture(null);         
    }
```

### <a name="receive-messages-from-the-individual-subscriptions"></a>从各个订阅接收消息

`receiveAllMessages()` 方法调用 `receiveAllMessageFromSubscription()` 方法，后者为每个调用创建一个订阅客户端并从各个订阅接收消息：

```java
public void receiveAllMessages() throws Exception {     
    System.out.printf("\nStart Receiving Messages.\n");
    
    CompletableFuture.allOf(
            receiveAllMessageFromSubscription(Subscriptions[0]),
            receiveAllMessageFromSubscription(Subscriptions[1]),
            receiveAllMessageFromSubscription(Subscriptions[2]) 
            ).join();
}

public CompletableFuture<Void> receiveAllMessageFromSubscription(String subscription) throws Exception {
        
        int receivedMessages = 0;

        // Create subscription client.
        IMessageReceiver subscriptionClient = ClientFactory.createMessageReceiverFromConnectionStringBuilder(new ConnectionStringBuilder(ConnectionString, TopicName+"/subscriptions/"+ subscription), ReceiveMode.PEEKLOCK);

        // Create a receiver from the subscription client and receive all messages.
        System.out.printf("\nReceiving messages from subscription %s.\n\n", subscription);

        while (true)
        {
            // This will make the connection wait for N seconds if new messages are available. 
            // If no additional messages come we close the connection. This can also be used to realize long polling.
            // In case of long polling you would obviously set it more to e.g. 60 seconds.
            IMessage receivedMessage = subscriptionClient.receive(Duration.ofSeconds(1));
            if (receivedMessage != null)
            {
                if ( receivedMessage.getProperties() != null ) {                                                                                
                    System.out.printf("StoreId=%s\n", receivedMessage.getProperties().get("StoreId"));                                                                                          
                    
                    // Show the label modified by the rule action
                    if(receivedMessage.getLabel() != null)
                        System.out.printf("Label=%s\n", receivedMessage.getLabel());   
                }
                
                byte[] body = receivedMessage.getBody();
                Item theItem = GSON.fromJson(new String(body, UTF_8), Item.class);
                System.out.printf("Item data. Price=%f, Color=%s, Category=%s\n", theItem.getPrice(), theItem.getColor(), theItem.getItemCategory());                          
                
                subscriptionClient.complete(receivedMessage.getLockToken());
                receivedMessages++;
            }
            else
            {
                // No more messages to receive.
                subscriptionClient.close();
                break;
            }
        }
        System.out.printf("\nReceived %s messages from subscription %s.\n", receivedMessages, subscription);
        
        return new CompletableFuture().completedFuture(null);
}
```

## <a name="next-steps"></a>后续步骤

在本教程中，你已使用 Azure CLI 预配了资源，然后从服务总线主题及其订阅发送并接收了消息。 你已了解如何：

> [!div class="checklist"]
> * 使用 Azure 门户创建一个服务总线主题和一个或多个对该主题的订阅
> * 使用 .NET 代码添加主题筛选器
> * 创建两条具有不同内容的消息
> * 发送消息并验证它们是否已到达预期的订阅
> * 从订阅接收消息

若要通过更多示例来了解如何发送和接收消息，请从 [GitHub 上的服务总线示例](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted)着手。

若要详细了解如何使用服务总线的发布/订阅功能，请转到下一教程。

> [!div class="nextstepaction"]
> [使用 PowerShell 和主题/订阅更新库存](service-bus-tutorial-topics-subscriptions-portal.md)

[试用帐户]: https://www.azure.cn/pricing/1rmb-trial
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[Install the Azure CLI]: https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest
[az group create]: https://docs.azure.cn/zh-cn/cli/group?view=azure-cli-latest#az_group_create