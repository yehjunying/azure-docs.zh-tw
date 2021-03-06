---
title: 開始使用 Azure 服務匯流排佇列 | Microsoft Docs
description: 在本教學課程中，您會建立 .NET Core 主控台應用程式，以在服務匯流排佇列中傳送和接收訊息。
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 5718106aee0e60d111398efdb839945c2c7a8a06
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "77471732"
---
# <a name="get-started-with-service-bus-queues"></a>開始使用服務匯流排佇列
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]
在本教學課程中，您會建立 .NET Core 主控台應用程式，以在服務匯流排佇列中傳送和接收訊息。

## <a name="prerequisites"></a>Prerequisites

- [Visual Studio 2019](https://www.visualstudio.com/vs)。
- [.NET Core SDK](https://www.microsoft.com/net/download/windows)，2.0 版或更新版本。
- Azure 訂用帳戶。 若要完成此教學課程，您需要 Azure 帳戶。 您可以[啟用自己的 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF)或是[註冊免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)。
- 如果您沒有可用的佇列，請執行[使用 Azure 入口網站建立服務匯流排佇列](service-bus-quickstart-portal.md)一文中的步驟，以建立佇列。

  - 閱讀服務匯流排佇列的快速概觀。
  - 建立服務匯流排命名空間。
  - 取得連接字串。
  - 建立服務匯流排佇列。

## <a name="send-messages-to-the-queue"></a>將訊息傳送到佇列

為了將訊息傳送到佇列，請使用 Visual Studio 撰寫 C# 主控台應用程式。

### <a name="create-a-console-application"></a>建立主控台應用程式

啟動 Visual Studio，並為 c # 建立新的**主控台應用程式（.Net Core）** 專案。 這個範例會將應用程式命名為*CoreSenderApp*。

### <a name="add-the-service-bus-nuget-package"></a>新增服務匯流排 NuGet 封裝

1. 以滑鼠右鍵按一下新建立的專案，然後選取 [管理 NuGet 套件]****。
1. 選取 [瀏覽]。  搜尋並選取 [ **[Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/)**]。
1. 選取 [**安裝**] 以完成安裝，然後關閉 [NuGet 套件管理員]。

    ![選取 NuGet 封裝][nuget-pkg]

### <a name="write-code-to-send-messages-to-the-queue"></a>撰寫程式碼以將訊息傳送到佇列

1. 在*Program.cs*中，于命名`using`空間定義的頂端新增下列語句（在類別宣告之前）：

    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

1. 在`Program`類別中，宣告下列變數：

    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ```

    輸入命名空間的連接字串作為`ServiceBusConnectionString`變數。 輸入您的佇列名稱。

1. 將`Main()`方法取代為下列**非同步** `Main`方法。 它會呼叫`SendMessagesAsync()`您將在下一個步驟中新增的方法，以將訊息傳送至佇列。 

    ```csharp
    public static async Task Main(string[] args)
    {    
        const int numberOfMessages = 10;
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after sending all the messages.");
        Console.WriteLine("======================================================");

        // Send messages.
        await SendMessagesAsync(numberOfMessages);

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```
1. 直接在`MainAsync()`方法後面，新增下列`SendMessagesAsync()`方法，以執行傳送指定的訊息數目`numberOfMessagesToSend` （目前設定為10）的工作：

    ```csharp
    static async Task SendMessagesAsync(int numberOfMessagesToSend)
    {
        try
        {
            for (var i = 0; i < numberOfMessagesToSend; i++)
            {
                // Create a new message to send to the queue.
                string messageBody = $"Message {i}";
                var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                // Write the body of the message to the console.
                Console.WriteLine($"Sending message: {messageBody}");

                // Send the message to the queue.
                await queueClient.SendAsync(message);
            }
        }
        catch (Exception exception)
        {
            Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
        }
    }
    ```

以下是您的*Program.cs*檔案應該看起來的樣子。

```csharp
namespace CoreSenderApp
{
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;

    class Program
    {
        // Connection String for the namespace can be obtained from the Azure portal under the 
        // 'Shared Access policies' section.
        const string ServiceBusConnectionString = "<your_connection_string>";
        const string QueueName = "<your_queue_name>";
        static IQueueClient queueClient;

        public static async Task Main(string[] args)
        {    
            const int numberOfMessages = 10;
            queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    
            Console.WriteLine("======================================================");
            Console.WriteLine("Press ENTER key to exit after sending all the messages.");
            Console.WriteLine("======================================================");
    
            // Send messages.
            await SendMessagesAsync(numberOfMessages);
    
            Console.ReadKey();
    
            await queueClient.CloseAsync();
        }

        static async Task SendMessagesAsync(int numberOfMessagesToSend)
        {
            try
            {
                for (var i = 0; i < numberOfMessagesToSend; i++)
                {
                    // Create a new message to send to the queue
                    string messageBody = $"Message {i}";
                    var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                    // Write the body of the message to the console
                    Console.WriteLine($"Sending message: {messageBody}");

                    // Send the message to the queue
                    await queueClient.SendAsync(message);
                }
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
            }
        }
    }
}
```

執行程式，並檢查 Azure 入口網站。

在 [命名空間**總覽**] 視窗中選取您的佇列名稱，以顯示 [佇列] [**基本**]。

![以計數和大小接收的訊息][queue-message]

佇列的作用中**訊息計數**值現在是**10**。 每次您執行此傳送者應用程式時，若未抓取訊息，此值會增加10。

每次應用程式將訊息新增至佇列時，佇列目前的大小會增加**基本**資訊中的**目前**值。

下一節將說明如何取得這些訊息。

## <a name="receive-messages-from-the-queue"></a>從佇列接收訊息

若要接收您所傳送的訊息，請建立另一個**主控台應用程式（.Net Core）** 應用程式。 如您對傳送者應用程式所做的一樣，安裝**Microsoft Azure。**

### <a name="write-code-to-receive-messages-from-the-queue"></a>撰寫程式碼以從佇列接收訊息

1. 在*Program.cs*中，于命名`using`空間定義的頂端新增下列語句（在類別宣告之前）：

    ```csharp
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

1. 在`Program`類別中，宣告下列變數：

    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string QueueName = "<your_queue_name>";
    static IQueueClient queueClient;
    ```

    輸入命名空間的連接字串作為`ServiceBusConnectionString`變數。 輸入您的佇列名稱。

1. 以下列程式碼取代 `Main()` 方法：

    ```csharp
    static void Main(string[] args)
    {
        MainAsync().GetAwaiter().GetResult();
    }

    static async Task MainAsync()
    {
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
        Console.WriteLine("======================================================");

        // Register QueueClient's MessageHandler and receive messages in a loop
        RegisterOnMessageHandlerAndReceiveMessages();

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```

1. 直接在`MainAsync()`方法後面，新增下列方法，以註冊訊息處理常式，並接收傳送者應用程式所傳送的訊息：

    ```csharp
    static void RegisterOnMessageHandlerAndReceiveMessages()
    {
        // Configure the message handler options in terms of exception handling, number of concurrent messages to deliver, etc.
        var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
        {
            // Maximum number of concurrent calls to the callback ProcessMessagesAsync(), set to 1 for simplicity.
            // Set it according to how many messages the application wants to process in parallel.
            MaxConcurrentCalls = 1,

            // Indicates whether the message pump should automatically complete the messages after returning from user callback.
            // False below indicates the complete operation is handled by the user callback as in ProcessMessagesAsync().
            AutoComplete = false
        };

        // Register the function that processes messages.
        queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    }
    ```

1. 直接在前一個方法後面，新增下列 `ProcessMessagesAsync()` 方法，以處理收到的訊息：

    ```csharp
    static async Task ProcessMessagesAsync(Message message, CancellationToken token)
    {
        // Process the message.
        Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

        // Complete the message so that it is not received again.
        // This can be done only if the queue Client is created in ReceiveMode.PeekLock mode (which is the default).
        await queueClient.CompleteAsync(message.SystemProperties.LockToken);

        // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
        // If queueClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
        // to avoid unnecessary exceptions.
    }
    ```

1. 最後，新增下列方法以處理任何可能發生的例外狀況：

    ```csharp
    // Use this handler to examine the exceptions received on the message pump.
    static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
    {
        Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
        var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
        Console.WriteLine("Exception context for troubleshooting:");
        Console.WriteLine($"- Endpoint: {context.Endpoint}");
        Console.WriteLine($"- Entity Path: {context.EntityPath}");
        Console.WriteLine($"- Executing Action: {context.Action}");
        return Task.CompletedTask;
    }
    ```

您的*Program.cs*檔案看起來應該像這樣：

```csharp
namespace CoreReceiverApp
{
    using System;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;

    class Program
    {
        // Connection String for the namespace can be obtained from the Azure portal under the 
        // 'Shared Access policies' section.
        const string ServiceBusConnectionString = "<your_connection_string>";
        const string QueueName = "<your_queue_name>";
        static IQueueClient queueClient;

        static void Main(string[] args)
        {
            MainAsync().GetAwaiter().GetResult();
        }

        static async Task MainAsync()
        {
            queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

            Console.WriteLine("======================================================");
            Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
            Console.WriteLine("======================================================");

            // Register QueueClient's MessageHandler and receive messages in a loop
            RegisterOnMessageHandlerAndReceiveMessages();
 
            Console.ReadKey();

            await queueClient.CloseAsync();
        }

        static void RegisterOnMessageHandlerAndReceiveMessages()
        {
            // Configure the MessageHandler Options in terms of exception handling, number of concurrent messages to deliver etc.
            var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
            {
                // Maximum number of Concurrent calls to the callback `ProcessMessagesAsync`, set to 1 for simplicity.
                // Set it according to how many messages the application wants to process in parallel.
                MaxConcurrentCalls = 1,

                // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
                // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
                AutoComplete = false
            };

            // Register the function that will process messages
            queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
        }

        static async Task ProcessMessagesAsync(Message message, CancellationToken token)
        {
            // Process the message
            Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

            // Complete the message so that it is not received again.
            // This can be done only if the queueClient is created in ReceiveMode.PeekLock mode (which is default).
            await queueClient.CompleteAsync(message.SystemProperties.LockToken);

            // Note: Use the cancellationToken passed as necessary to determine if the queueClient has already been closed.
            // If queueClient has already been Closed, you may chose to not call CompleteAsync() or AbandonAsync() etc. calls 
            // to avoid unnecessary exceptions.
        }

        static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
        {
            Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
            var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
            Console.WriteLine("Exception context for troubleshooting:");
            Console.WriteLine($"- Endpoint: {context.Endpoint}");
            Console.WriteLine($"- Entity Path: {context.EntityPath}");
            Console.WriteLine($"- Executing Action: {context.Action}");
            return Task.CompletedTask;
        }
    }
}
```

執行程式，並再次檢查入口網站。 作用中**訊息計數**和**目前**值現在是**0**。

![接收訊息之後的佇列][queue-message-receive]

恭喜！ 您現在已建立佇列、將一組訊息傳送至該佇列，以及從相同的佇列接收這些訊息。

> [!NOTE]
> 您可以使用[服務匯流排總管](https://github.com/paolosalvatori/ServiceBusExplorer/)來管理服務匯流排資源。 服務匯流排 Explorer 可讓使用者輕鬆地連接到服務匯流排命名空間，並管理訊息實體。 此工具提供匯入/匯出功能，或測試主題、佇列、訂用帳戶、轉送服務、通知中樞和事件中樞的能力等先進功能。

## <a name="next-steps"></a>後續步驟

查看 [GitHub 存放庫以及範例](https://github.com/Azure/azure-service-bus/tree/master/samples)，其中會展示一些更進階的服務匯流排傳訊功能。

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/messages-sent-to-essentials.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive-in-essentials.png

