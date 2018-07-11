---
title: Enviar eventos para Hubs de Eventos do Azure com o .NET Standard | Microsoft Docs
description: Introdução ao envio de eventos para Hubs de Eventos no .NET Standard
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2018
ms.author: sethm
ms.openlocfilehash: 3dba92467dfaf377236a25f48899a8a53c587a82
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130978"
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>Introdução ao envio de mensagens para os Hubs de Eventos do Azure no .NET Standard

> [!NOTE]
> Este exemplo está disponível no [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).

Este tutorial mostra como escrever uma aplicação de consola .NET Core que envia um conjunto de mensagens para um hub de eventos. Pode executar a solução do [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) conforme está, ao substituir as cadeias `EhConnectionString` e `EhEntityPath` pelos valores do hub de eventos. Ou pode seguir os passos neste tutorial para criar a sua própria.

## <a name="prerequisites"></a>Pré-requisitos

* [Microsoft Visual Studio 2015 ou 2017](http://www.visualstudio.com). Os exemplos neste tutorial utilizam o Visual Studio 2017, mas o Visual Studio 2015 também é suportado.
* [Ferramentas do .NET Core Visual Studio 2015 ou 2017](http://www.microsoft.com/net/core).
* Uma subscrição do Azure.
* [Um espaço de nomes do hub de eventos e um hub de eventos](event-hubs-quickstart-portal.md).

Para enviar mensagens para um hub de eventos, este tutorial utiliza o Visual Studio para escrever uma aplicação de consola C#.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Criar um espaço de nomes de Hubs de Eventos e um hub de eventos

Para criar um espaço de nomes e um hub de eventos, siga o procedimento descrito [neste artigo](event-hubs-quickstart-portal.md) e, em seguida, continue com este tutorial.

## <a name="create-a-console-application"></a>Criar uma aplicação de consola

Inicie o Visual Studio. No menu **Ficheiro**, clique em **Novo** e, em seguida, clique em **Projeto**. Criar uma aplicação de consola .NET Core.

![Novo projeto][1]

## <a name="add-the-event-hubs-nuget-package"></a>Adicionar o pacote NuGet dos Hubs de Eventos

Adicione o pacote NuGet da biblioteca .NET Standard [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) ao seu projeto, através destes passos: 

1. Clique com o botão direito do rato no projeto recém-criado e selecione **Gerir Pacotes NuGet**.
2. Clique no separador **Procurar** e, em seguida, procure "Microsoft.Azure.EventHubs" e, em seguida, selecione o pacote **Microsoft.Azure.EventHubs**. Clique em **Instalar** para concluir a instalação e, em seguida, feche esta caixa de diálogo.

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a>Escrever alguns códigos para enviar mensagens ao hub de eventos

1. Adicione as seguinte declarações `using` à parte superior do ficheiro Program.cs:

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Adicione constantes à classe `Program` para a cadeia de ligação dos Hubs de Eventos e o caminho da entidade (nome do hub de eventos individual). Substitua os marcadores de posição entre parênteses retos pelos valores adequados que foram obtidos ao criar o hub de eventos. Certifique-se de que `{Event Hubs connection string}` é a cadeia de ligação ao nível do espaço de nomes e não é a cadeia de hub de eventos. 

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Adicione um método novo denominado `MainAsync` à classe `Program`, da seguinte forma:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but this simple scenario
        // uses the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. Adicione um método novo denominado `SendMessagesToEventHub` à classe `Program`, da seguinte forma:

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. Adicione o seguinte código ao método `Main` na classe `Program`:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   O ficheiro Program.cs deve ter o seguinte aspeto.

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EventHubConnectionString = "{Event Hubs connection string}";
            private const string EventHubName = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
                {
                    EntityPath = EventHubName
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. Execute o programa e certifique-se de que não existem erros.

Parabéns! Enviou agora mensagens para um hub de eventos.

## <a name="next-steps"></a>Passos seguintes
Pode saber mais sobre os Hubs de Eventos nas seguintes ligações:

* [Receber eventos dos Hubs de Eventos](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Descrição geral dos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Criar um hub de eventos](event-hubs-create.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcoresnd.png
