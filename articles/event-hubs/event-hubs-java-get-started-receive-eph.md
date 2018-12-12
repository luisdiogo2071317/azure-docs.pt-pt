---
title: Receber eventos com Java - Event Hubs do Azure | Documentos da Microsoft
description: Este artigo fornece um passo a passo para criar uma aplicação de Java que recebe eventos dos Hubs de eventos do Azure.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: e29cf43f490bf5e8bac5e5c36b16476f93d80bfa
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53081966"
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a>Receber eventos dos Hubs de eventos do Azure com Java

Os Hubs de eventos é um sistema de ingestão altamente dimensionável, que pode ingerir milhões de eventos por segundo que uma aplicação possa processar e analisar enormes quantidades de dados produzidos pelos seus dispositivos e aplicações ligados. Depois de recolhidos nos Event Hubs, pode transformar e armazenar dados em qualquer fornecedor de análise em tempo real ou cluster de armazenamento.

Para obter mais informações, consulte a [descrição geral dos Hubs de eventos][Event Hubs overview].

Este tutorial mostra como receber eventos para um hub de eventos utilizando uma aplicação de consola escrita em Java.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, terá dos seguintes pré-requisitos:

* Um ambiente de desenvolvimento do Java. Para este tutorial, partimos do princípio [Eclipse](https://www.eclipse.org/).
* Uma conta ativa do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita][] antes de começar.

O código neste tutorial se baseia a [EventProcessorSample código no GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/EventProcessorSample), que pode examinar para ver toda a aplicação de trabalho.

## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Receber mensagens com o EventProcessorHost em Java

**EventProcessorHost** é uma classe de Java que simplifica a receção de eventos provenientes dos Hubs de eventos ao gerir pontos de verificação persistentes e receções em paralelo desses Hubs de eventos. Se utilizar o EventProcessorHost, pode dividir eventos por vários recetores, mesmo se estiverem alojados em nós diferentes. Este exemplo mostra como utilizar o EventProcessorHost para um recetor único.

### <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

Para utilizar o EventProcessorHost, tem de ter uma [conta de armazenamento do Azure][Azure Storage account]:

1. Inicie sessão para o [portal do Azure][Azure portal]e clique em **+ criar um recurso** no lado esquerdo do ecrã.
2. Clique em **Armazenamento** e, em seguida, clique em **Conta de armazenamento**. Na **criar conta de armazenamento** janela, escreva um nome para a conta de armazenamento. Conclua o resto dos campos, selecione a região pretendida e, em seguida, clique em **criar**.
   
    ![Criar conta de armazenamento](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Clique na conta de armazenamento recentemente criada e, em seguida, clique em **chaves de acesso**:
   
    ![Obter chaves de acesso](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Copie o valor de chave1 para uma localização temporária. Vai utilizá-lo mais tarde no tutorial.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>Criar um projeto Java com o EventProcessorHost

A biblioteca de cliente de Java dos Hubs de eventos está disponível para uso em projetos Maven a partir da [repositório Central Maven][Maven Package]e pode ser referenciada com a seguinte declaração de dependência dentro do Maven arquivo de projeto. A versão atual para artefactos do azure-eventhubs-eph é 2.0.1 e a versão atual para o azure-eventhubs artefacto é 1.0.2:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.2</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>2.0.1</version>
</dependency>
```

Para diferentes tipos de ambientes de compilação, pode obter explicitamente os mais recente ficheiros JAR dos [repositório Central Maven][Maven Package].  

1. Para o exemplo que se segue, comece por criar um novo projeto Maven para uma consola/aplicação shell no seu ambiente de desenvolvimento Java favorito. A classe é chamada `ErrorNotificationHandler`.     
   
    ```java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. Utilize o seguinte código para criar uma nova classe designada `EventProcessorSample`. Substitua os marcadores de posição pelos valores utilizados quando criou a conta de armazenamento e do hub de eventos:
   
   ```java
   package com.microsoft.azure.eventhubs.samples.eventprocessorsample;

   import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
   import com.microsoft.azure.eventhubs.EventData;
   import com.microsoft.azure.eventprocessorhost.CloseReason;
   import com.microsoft.azure.eventprocessorhost.EventProcessorHost;
   import com.microsoft.azure.eventprocessorhost.EventProcessorOptions;
   import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   import com.microsoft.azure.eventprocessorhost.IEventProcessor;
   import com.microsoft.azure.eventprocessorhost.PartitionContext;

   import java.util.concurrent.ExecutionException;
   import java.util.function.Consumer;

   public class EventProcessorSample
   {
       public static void main(String args[]) throws InterruptedException, ExecutionException
       {
           String consumerGroupName = "$Default";
           String namespaceName = "----NamespaceName----";
           String eventHubName = "----EventHubName----";
           String sasKeyName = "----SharedAccessSignatureKeyName----";
           String sasKey = "----SharedAccessSignatureKey----";
           String storageConnectionString = "----AzureStorageConnectionString----";
           String storageContainerName = "----StorageContainerName----";
           String hostNamePrefix = "----HostNamePrefix----";
        
           ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder()
                .setNamespaceName(namespaceName)
                .setEventHubName(eventHubName)
                .setSasKeyName(sasKeyName)
                .setSasKey(sasKey);
        
           EventProcessorHost host = new EventProcessorHost(
                EventProcessorHost.createHostName(hostNamePrefix),
                eventHubName,
                consumerGroupName,
                eventHubConnectionString.toString(),
                storageConnectionString,
                storageContainerName);
        
           System.out.println("Registering host named " + host.getHostName());
           EventProcessorOptions options = new EventProcessorOptions();
           options.setExceptionNotification(new ErrorNotificationHandler());

           host.registerEventProcessor(EventProcessor.class, options)
           .whenComplete((unused, e) ->
           {
               if (e != null)
               {
                   System.out.println("Failure while registering: " + e.toString());
                   if (e.getCause() != null)
                   {
                       System.out.println("Inner exception: " + e.getCause().toString());
                   }
               }
           })
           .thenAccept((unused) ->
           {
               System.out.println("Press enter to stop.");
               try 
               {
                   System.in.read();
               }
               catch (Exception e)
               {
                   System.out.println("Keyboard read failed: " + e.toString());
               }
           })
           .thenCompose((unused) ->
           {
               return host.unregisterEventProcessor();
           })
           .exceptionally((e) ->
           {
               System.out.println("Failure while unregistering: " + e.toString());
               if (e.getCause() != null)
               {
                   System.out.println("Inner exception: " + e.getCause().toString());
               }
               return null;
           })
           .get(); // Wait for everything to finish before exiting main!
        
           System.out.println("End of sample");
       }
    ```
3. Criar uma classe mais chamada `EventProcessor`, usando o seguinte código:
   
    ```java
    public static class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        // OnOpen is called when a new event processor instance is created by the host. 
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        // OnClose is called when an event processor instance is being shut down. 
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        // onError is called when an error occurs in EventProcessorHost code that is tied to this partition, such as a receiver failure.
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        // onEvents is called when events are received on this partition of the Event Hub. 
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> events) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got event batch");
            int eventCount = 0;
            for (EventData data : events)
            {
                try
                {
                    System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                            data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBytes(), "UTF8"));
                    eventCount++;
                    
                    // Checkpointing persists the current position in the event stream for this partition and means that the next
                    // time any host opens an event processor on this event hub+consumer group+partition combination, it will start
                    // receiving at the event after this one. 
                    this.checkpointBatchingCount++;
                    if ((checkpointBatchingCount % 5) == 0)
                    {
                        System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                            data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                        // Checkpoints are created asynchronously. It is important to wait for the result of checkpointing
                        // before exiting onEvents or before creating the next checkpoint, to detect errors and to ensure proper ordering.
                        context.checkpoint(data).get();
                    }
                }
                catch (Exception e)
                {
                    System.out.println("Processing failed for an event: " + e.toString());
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + eventCount + " for host " + context.getOwner());
        }
    }
    ```

Este tutorial utiliza uma única instância do EventProcessorHost. Para aumentar o débito, recomendamos que execute várias instâncias do EventProcessorHost, preferencialmente, em computadores separados.  Ele fornece também a redundância. Nesses casos, as várias instâncias coordenam-se automaticamente entre si para equilibrarem a carga dos eventos recebidos. Se pretender várias recetores para que cada um processe *todos* os eventos, terá de utilizar o conceito **ConsumerGroup**. Se receber eventos de vários computadores, poderá ser útil especificar os nomes das instâncias do EventProcessorHost com base nos computadores (ou funções) em que estão implementadas.

## <a name="publishing-messages-to-eventhub"></a>Publicação de mensagens para EventHub

Antes das mensagens são obtidas pelos consumidores, têm de ser publicado para as partições primeiro pelos fabricantes. Vale a pena observar que quando as mensagens são publicadas para o hub de eventos sincronicamente usando o método de sendSync() no objeto com.microsoft.azure.eventhubs.EventHubClient, a mensagem pode ser enviada para uma partição específica ou distribuída para todas as partições disponíveis de uma forma de rodízio consoante esteja a chave de partição está especificada ou não.

Quando uma cadeia de caracteres que representa a chave de partição for especificada, a chave é protegido por hash para determinar qual partição para enviar o evento.

Quando a chave de partição não está definida, em seguida, as mensagens são round robined para todas as partições disponíveis

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

```

## <a name="implementing-a-custom-checkpointmanager-for-eventprocessorhost-eph"></a>Implementando um CheckpointManager personalizado para o EventProcessorHost (EPH)

A API fornece um mecanismo para implementar o seu Gestor de ponto de verificação personalizada para cenários em que a implementação padrão não é compatível com o seu caso de utilização.

O Gestor de ponto de verificação padrão utiliza o armazenamento de BLOBs, mas se substituir o Gestor de ponto de verificação utilizado por EPH com sua própria implementação, pode utilizar qualquer armazenamento que pretende criar a sua implementação do Gestor de ponto de verificação.

Criar uma classe que implementa a interface com.microsoft.azure.eventprocessorhost.ICheckpointManager

Utilizar a sua implementação personalizada do Gestor de ponto de verificação (com.microsoft.azure.eventprocessorhost.ICheckpointManager)

Dentro de sua implementação, pode substituir o mecanismo de ponto de verificação padrão e implementar nossos própria pontos de verificação com base no seu próprio arquivo de dados (SQL Server, cosmos DB, Cache do Azure para Redis etc). Recomendamos que o arquivo utilizado para fazer uma cópia de sua implementação do Gestor de ponto de verificação está acessível a todas as instâncias EPH que estão a processar os eventos para o grupo de consumidores.

Pode usar qualquer arquivo de dados que está disponível no seu ambiente.

A classe com.microsoft.azure.eventprocessorhost.EventProcessorHost oferece dois construtores que permitem que substitua o Gestor de ponto de verificação para o EventProcessorHost.

## <a name="next-steps"></a>Passos Seguintes
Neste início rápido, criou uma aplicação de Java que recebeu mensagens a partir de um hub de eventos. Para saber como enviar eventos para um hub de eventos com Java, veja [enviar eventos do hub de eventos - Java](event-hubs-java-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
[conta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
