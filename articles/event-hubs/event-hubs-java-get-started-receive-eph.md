---
title: Receber eventos de linguagem Java de Event Hubs do Azure | Microsoft Docs
description: Começar a receber de Event Hubs utilizando Java
services: event-hubs
documentationcenter: ''
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 38e3be53-251c-488f-a856-9a500f41b6ca
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2018
ms.author: sethm
ms.openlocfilehash: bf87bed80c142bce6229ad858a33a1c6ede63a23
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a>Receber eventos de linguagem Java de Event Hubs do Azure

Os Event Hubs são um sistema de ingestão altamente dimensionável, que pode ingerir milhões de eventos por segundo, que uma aplicação possa processar e analisar as quantidades enormes de dados produzidos pelos seus dispositivos e aplicações ligados. Depois de recolhidos nos Event Hubs, pode transformar e armazenar dados em qualquer fornecedor de análise em tempo real ou cluster de armazenamento.

Para obter mais informações, consulte o [descrição geral dos Event Hubs][Event Hubs overview].

Este tutorial mostra como receber eventos para um hub de eventos utilizando uma aplicação de consola escrita em Java.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, terá dos seguintes pré-requisitos:

* Ambiente de desenvolvimento Java. Para este tutorial, partimos do pressuposto [Eclipse](https://www.eclipse.org/).
* Uma conta ativa do Azure. Se não tiver uma subscrição do Azure, crie um [conta gratuita][] antes de começar.

O código neste tutorial baseia-se no [EventProcessorSample código no GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/EventProcessorSample), que pode examinar para ver o completo aplicação de trabalho.

## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Receber mensagens com o EventProcessorHost em Java

**EventProcessorHost** é uma classe do Java que simplifica a receção de eventos provenientes dos Event Hubs ao gerir pontos de verificação persistentes e receções em paralelo desses Event hubs. Se utilizar o EventProcessorHost, pode dividir eventos por vários recetores, mesmo quando alojados em nós diferentes. Este exemplo mostra como utilizar o EventProcessorHost para um recetor único.

### <a name="create-a-storage-account"></a>Criar uma conta do Storage

Para utilizar o EventProcessorHost, tem de ter um [conta de armazenamento do Azure][Azure Storage account]:

1. Inicie sessão no [portal do Azure][Azure portal]e clique em **+ criar um recurso** no lado esquerdo do ecrã.
2. Clique em **Armazenamento** e, em seguida, clique em **Conta de armazenamento**. No **criar conta de armazenamento** janela, escreva um nome para a conta de armazenamento. Conclua o resto dos campos, selecione a região pretendida e, em seguida, clique em **criar**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Clique na conta de armazenamento acabada de criar e, em seguida, clique em **chaves de acesso**:
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Copie o valor de chave1 para uma localização temporária, para utilizar mais tarde neste tutorial.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>Criar um projeto Java com o EventProcessorHost

A biblioteca de cliente de Java para os Event Hubs está disponível para utilização em projetos Maven a partir de [repositório Central Maven][Maven Package]e podem ser referenciados utilizando a seguinte declaração de dependência dentro do Maven ficheiro de projeto. A versão atual é 1.0.0:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>1.0.0</version>
</dependency>
```

Para diferentes tipos de ambientes de compilação, pode obter explicitamente os mais recente ficheiros JAR do [repositório Central Maven][Maven Package].  

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
2. Utilize o seguinte código para criar uma nova classe designada `EventProcessorSample`. Substitua os marcadores de posição com os valores que utilizou quando criou a conta de armazenamento e de hub de eventos:
   
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
3. Criar uma classe mais chamada `EventProcessor`, utilizando o seguinte código:
   
    ```java
    public static class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        // OnOpen is called when a new event processor instance is created by the host. In a real implementation, this
        // is the place to do initialization so that events can be processed when they arrive, such as opening a database
        // connection.
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        // OnClose is called when an event processor instance is being shut down. The reason argument indicates whether the shut down
        // is because another host has stolen the lease for this partition or due to error or host shutdown. In a real implementation,
        // this is the place to do cleanup for resources that were opened in onOpen.
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        // onError is called when an error occurs in EventProcessorHost code that is tied to this partition, such as a receiver failure.
        // It is NOT called for exceptions thrown out of onOpen/onClose/onEvents. EventProcessorHost is responsible for recovering from
        // the error, if possible, or shutting the event processor down if not, in which case there will be a call to onClose. The
        // notification provided to onError is primarily informational.
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        // onEvents is called when events are received on this partition of the Event Hub. The maximum number of events in a batch
        // can be controlled via EventProcessorOptions. Also, if the "invoke processor after receive timeout" option is set to true,
        // this method will be called with null when a receive timeout occurs.
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> events) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got event batch");
            int eventCount = 0;
            for (EventData data : events)
            {
                // It is important to have a try-catch around the processing of each event. Throwing out of onEvents deprives
                // you of the chance to process any remaining events in the batch. 
                try
                {
                    System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                            data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBytes(), "UTF8"));
                    eventCount++;
                    
                    // Checkpointing persists the current position in the event stream for this partition and means that the next
                    // time any host opens an event processor on this event hub+consumer group+partition combination, it will start
                    // receiving at the event after this one. Checkpointing is usually not a fast operation, so there is a tradeoff
                    // between checkpointing frequently (to minimize the number of events that will be reprocessed after a crash, or
                    // if the partition lease is stolen) and checkpointing infrequently (to reduce the impact on event processing
                    // performance). Checkpointing every five events is an arbitrary choice for this sample.
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

> [!NOTE]
> Este tutorial utiliza uma única instância do EventProcessorHost. Para aumentar o débito, recomenda-se que execute várias instâncias do EventProcessorHost, preferencialmente, em computadores separados.  Isto fornece redundância bem. Nesses casos, as várias instâncias coordenam-se automaticamente entre si para equilibrarem a carga dos eventos recebidos. Se pretender várias recetores para que cada um processe *todos* os eventos, terá de utilizar o conceito **ConsumerGroup**. Se receber eventos de vários computadores, poderá ser útil especificar os nomes das instâncias do EventProcessorHost com base nos computadores (ou funções) em que estão implementadas.
> 
> 

## <a name="next-steps"></a>Passos Seguintes
Pode saber mais sobre os Hubs de Eventos ao aceder às seguintes ligações:

* [Descrição geral dos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Criar um Hub de Eventos](event-hubs-create.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
[conta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio