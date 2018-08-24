---
title: Receber eventos dos Hubs de eventos do Azure com Java | Documentos da Microsoft
description: Começar a receber de Hubs de eventos com Java
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 08/12/2018
ms.author: shvija
ms.openlocfilehash: 486dca6c4cc98b660e7d824b6f0646c06013011e
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746788"
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
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Clique na conta de armazenamento recentemente criada e, em seguida, clique em **chaves de acesso**:
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Copie o valor de chave1 para uma localização temporária, para utilizar mais tarde neste tutorial.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>Criar um projeto Java com o EventProcessorHost

A biblioteca de cliente de Java dos Hubs de eventos está disponível para uso em projetos Maven a partir da [repositório Central Maven][Maven Package]e pode ser referenciada com a seguinte declaração de dependência dentro do Maven arquivo de projeto. A versão atual for 1.0.0:    

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

Este tutorial utiliza uma única instância do EventProcessorHost. Para aumentar o débito, recomenda-se que execute várias instâncias do EventProcessorHost, preferencialmente, em computadores separados.  Esta opção fornece redundância também. Nesses casos, as várias instâncias coordenam-se automaticamente entre si para equilibrarem a carga dos eventos recebidos. Se pretender várias recetores para que cada um processe *todos* os eventos, terá de utilizar o conceito **ConsumerGroup**. Se receber eventos de vários computadores, poderá ser útil especificar os nomes das instâncias do EventProcessorHost com base nos computadores (ou funções) em que estão implementadas.

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