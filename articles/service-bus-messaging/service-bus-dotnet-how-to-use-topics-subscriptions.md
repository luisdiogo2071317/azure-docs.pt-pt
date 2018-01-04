---
title: "Introdução aos tópicos e subscrições do Azure Service Bus | Microsoft Docs"
description: "Grave uma aplicação da consola C# .NET Core que utilize tópicos e subscrições das mensagens do Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 12/6/2017
ms.author: sethm
ms.openlocfilehash: 5391951c34498b3bd684cd2e223b8288d4040247
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="get-started-with-service-bus-topics"></a>Introdução aos tópicos do Service Bus

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Este tutorial contém os seguintes passos:

1. Criar um espaço de nomes do Service Bus com o Portal do Azure.
2. Criar um tópico do Service Bus com o portal do Azure.
3. Criar uma subscrição do Service Bus com o portal do Azure.
4. Escreva uma aplicação de consola .NET Core para enviar um conjunto de mensagens para o tópico.
5. Escreva uma aplicação de consola .NET Core para receber essas mensagens da subscrição.

## <a name="prerequisites"></a>Pré-requisitos

1. [Visual Studio 2017 Atualização 3 (versão 15.3, 26730.01)](http://www.visualstudio.com/vs) ou posterior.
2. [SDK NET Core](https://www.microsoft.com/net/download/windows), versão 2.0 ou posterior.
2. Uma subscrição do Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Criar um espaço de nomes com o Portal do Azure

> [!NOTE] 
> Também pode criar um espaço de nomes do Service Bus e entidades de mensagens com o [PowerShell](/powershell/azure/get-started-azureps). Para obter mais informações, veja [Use PowerShell to manage Service Bus resources (Utilizar o PowerShell para gerir recursos do Service Bus)](service-bus-manage-with-ps.md).

Se já tiver criado um espaço de nomes das mensagens do Service Bus, avance para a secção [Criar um tópico com o portal do Azure](#2-create-a-topic-using-the-azure-portal).

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-the-azure-portal"></a>2. Criar um tópico com o portal do Azure

1. Inicie sessão no [Portal do Azure][azure-portal].
2. No painel de navegação à esquerda do portal, clique em **Service Bus** (se não vir **Service Bus**, clique em **Mais serviços** ou clique em **Todos os Recursos**). Clique no espaço de nomes no qual gostaria de criar o tópico. 
3. Será apresentada a janela de descrição geral do espaço de nomes. Clique em **Tópicos**:
   
    ![Criar um tópico][createtopic1]
4. Clique em **+ Tópico**.
   
    ![Selecionar tópicos][createtopic2]
5. Introduza um nome para o tópico. Deixe as outras opções com os valores predefinidos.
   
    ![Selecionar Novo][createtopic3]
6. Na parte inferior do diálogo, clique em **Criar**.

## <a name="3-create-a-subscription-to-the-topic"></a>3. Criar uma subscrição para o tópico

1. No painel de recursos do portal, clique no espaço de nomes que criou no passo 1 e clique em **Tópicos** e, em seguida, no nome do tópico que criou no passo 2.
2. Na parte superior do painel de descrição geral, clique em **+ Subscrição**, para adicionar uma subscrição a esse tópico.

    ![Criar subscrição][createtopic4]

3. Introduza um nome para a subscrição. Deixe as outras opções com os valores predefinidos.

## <a name="4-send-messages-to-the-topic"></a>4. Enviar mensagens para o tópico

Para enviar mensagens para o tópico, grave uma aplicação de consola C# com o Visual Studio.

### <a name="create-a-console-application"></a>Criar uma aplicação de consola

Abra o Visual Studio e crie um novo projeto de **Console App (.NET Core) (Aplicação de Consola (.NET Core))**.

### <a name="add-the-service-bus-nuget-package"></a>Adicionar o pacote NuGet do Service Bus

1. Clique com o botão direito do rato no projeto recém-criado e selecione **Gerir Pacotes NuGet**.
2. Clique no separador **Procurar**, procure **[Microsoft.Azure.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/)** e selecione o item **Microsoft.Azure.ServiceBus**. Clique em **Instalar** para concluir a instalação e, em seguida, feche esta caixa de diálogo.
   
    ![Selecionar um pacote NuGet][nuget-pkg]

### <a name="write-code-to-send-messages-to-the-topic"></a>Escrever código para enviar mensagens para o tópico

1. Em Program.cs, adicione as seguintes `using` declarações na parte superior da definição de espaço de nomes, antes da declaração de classe:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. Na classe `Program`, declare as variáveis seguintes. Defina a variável `ServiceBusConnectionString` para a cadeia de ligação que obteve ao criar o espaço de nomes e defina `TopicName` para o nome que utilizou ao criar o tópico:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string TopicName = "<your_topic_name>";
    static ITopicClient topicClient;
    ``` 

3. Substitua os conteúdos predefinidos de `Main()` com a seguinte linha de código:

    ```csharp
    MainAsync().GetAwaiter().GetResult();
    ```
   
4. Diretamente depois `Main()`, adicione o seguinte método `MainAsync()` assíncrono que chama o método de envio de mensagens:

    ```csharp
    static async Task MainAsync()
    {
        const int numberOfMessages = 10;
        topicClient = new TopicClient(ServiceBusConnectionString, TopicName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after sending all the messages.");
        Console.WriteLine("======================================================");

        // Send messages.
        await SendMessagesAsync(numberOfMessages);

        Console.ReadKey();

        await topicClient.CloseAsync();
    }
    ```

5. Imediatamente depois do método `MainAsync()`, adicione o seguinte método `SendMessagesAsync()` que realiza o trabalho de enviar o número de mensagens especificado pelo `numberOfMessagesToSend` (atualmente definido para 10):

    ```csharp
    static async Task SendMessagesAsync(int numberOfMessagesToSend)
    {
        try
        {
            for (var i = 0; i < numberOfMessagesToSend; i++)
            {
                // Create a new message to send to the topic.
                string messageBody = $"Message {i}";
                var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                // Write the body of the message to the console.
                Console.WriteLine($"Sending message: {messageBody}");

                // Send the message to the topic.
                await topicClient.SendAsync(message);
            }
        }
        catch (Exception exception)
        {
            Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
        }
    }
    ```
  
6. O ficheiro Program.cs do remetente deve ter o seguinte aspeto.
   
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
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string TopicName = "<your_topic_name>";
            static ITopicClient topicClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                const int numberOfMessages = 10;
                topicClient = new TopicClient(ServiceBusConnectionString, TopicName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after sending all the messages.");
                Console.WriteLine("======================================================");

                // Send messages.
                await SendMessagesAsync(numberOfMessages);

                Console.ReadKey();

                await topicClient.CloseAsync();
            }

            static async Task SendMessagesAsync(int numberOfMessagesToSend)
            {
                try
                {
                    for (var i = 0; i < numberOfMessagesToSend; i++)
                    {
                        // Create a new message to send to the topic
                        string messageBody = $"Message {i}";
                        var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                        // Write the body of the message to the console
                        Console.WriteLine($"Sending message: {messageBody}");

                        // Send the message to the topic
                        await topicClient.SendAsync(message);
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

3. Execute o programa e verifique o portal do Azure: clique no nome do seu tópico n janela **Descrição geral** do espaço de nomes. É apresentado o ecrã **Informações Básicas** do tópico. Na subscrição apresentada na parte inferior da janela, tenha em atenção que o valor da **Contagem de Mensagens** da subscrição é agora **10**. Sempre que executar a aplicação do remetente sem recuperar as mensagens (conforme descrito na próxima secção), esse valor aumenta 10. Note também que o tamanho atual do tópico aumenta o valor **Atual** na janela **Aspetos essenciais**, sempre que a aplicação adicionar uma mensagem ao tópico.
   
      ![Tamanho da mensagem][topic-message]

## <a name="5-receive-messages-from-the-subscription"></a>5. Receber mensagens da subscrição

Para receber as mensagens que acabou de enviar, crie outra aplicação de consola .NET Core e instale o pacote NuGet **Microsoft.Azure.ServiceBus**, semelhante à aplicação do remetente anterior.

### <a name="write-code-to-receive-messages-from-the-subscription"></a>Escrever código para receber mensagens da subscrição

1. Em Program.cs, adicione as seguintes `using` declarações na parte superior da definição de espaço de nomes, antes da declaração de classe:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. Na classe `Program`, declare as variáveis seguintes. Defina a variável `ServiceBusConnectionString` para a cadeia de ligação que obteve ao criar o espaço de nomes, defina `TopicName` para o nome que utilizou ao criar o tópico e defina `SubscriptionName` para o nome que utilizou ao criar a subscrição para o tópico:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string TopicName = "<your_topic_name>";
    const string SubscriptionName = "<your_subscription_name>";
    static ISubscriptionClient subscriptionClient;
    ```

3. Substitua os conteúdos predefinidos de `Main()` com a seguinte linha de código:

    ```csharp
    MainAsync().GetAwaiter().GetResult();
    ```

4. Diretamente depois `Main()`, adicione o seguinte método `MainAsync()` assíncrono que chama o método `RegisterOnMessageHandlerAndReceiveMessages()`:

    ```csharp
    static async Task MainAsync()
    {
        subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
        Console.WriteLine("======================================================");

        // Register subscription message handler and receive messages in a loop
        RegisterOnMessageHandlerAndReceiveMessages();

        Console.ReadKey();

        await subscriptionClient.CloseAsync();
    }
    ```

5. Imediatamente depois do método `MainAsync()`, adicione o seguinte método que regista o processador de mensagens e recebe as mensagens enviadas pela aplicação remetente:

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
        subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    }
    ```    

6. Diretamente depois do método anterior, adicione o seguinte método `ProcessMessagesAsync()` para processar as mensagens recebidas:
 
    ```csharp
    static async Task ProcessMessagesAsync(Message message, CancellationToken token)
    {
        // Process the message.
        Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

        // Complete the message so that it is not received again.
        // This can be done only if the subscriptionClient is created in ReceiveMode.PeekLock mode (which is the default).
        await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);

        // Note: Use the cancellationToken passed as necessary to determine if the subscriptionClient has already been closed.
        // If subscriptionClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
        // to avoid unnecessary exceptions.
    }
    ```

7. Por fim, adicione o seguinte método para processar quaisquer exceções que possam ocorrer:
 
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

8. O ficheiro Program.cs do recetor deve ter o seguinte aspeto:
   
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
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string TopicName = "<your_topic_name>";
            const string SubscriptionName = "<your_subscription_name>";
            static ISubscriptionClient subscriptionClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
                Console.WriteLine("======================================================");

                // Register subscription message handler and receive messages in a loop.
                RegisterOnMessageHandlerAndReceiveMessages();

                Console.ReadKey();

                await subscriptionClient.CloseAsync();
            }

            static void RegisterOnMessageHandlerAndReceiveMessages()
            {
                // Configure the message handler options in terms of exception handling, number of concurrent messages to deliver, etc.
                var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
                {
                    // Maximum number of concurrent calls to the callback ProcessMessagesAsync(), set to 1 for simplicity.
                    // Set it according to how many messages the application wants to process in parallel.
                    MaxConcurrentCalls = 1,

                    // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
                    // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
                    AutoComplete = false
                };

                // Register the function that processes messages.
                subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
            }

            static async Task ProcessMessagesAsync(Message message, CancellationToken token)
            {
                // Process the message.
                Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

                // Complete the message so that it is not received again.
                // This can be done only if the subscriptionClient is created in ReceiveMode.PeekLock mode (which is the default).
                await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);

                // Note: Use the cancellationToken passed as necessary to determine if the subscriptionClient has already been closed.
                // If subscriptionClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
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
9. Execute o programa e consulte novamente o portal. Tenha em atenção que agora o valor da **Contagem de Mensagens** e de **Atual** é **0**.
   
    ![Comprimento do tópico][topic-message-receive]

Parabéns! Através da biblioteca .NET Standard, criou um tópico e uma subscrição, enviou 10 mensagens e recebeu essas mensagens.

## <a name="next-steps"></a>Passos seguintes

Consulte o nosso [repositório do GitHub com exemplos](https://github.com/Azure/azure-service-bus/tree/master/samples) que demonstram algumas das funcionalidades mais avançadas das mensagens do Service Bus.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com
