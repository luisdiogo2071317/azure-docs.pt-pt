---
title: Enviar eventos para Hubs de eventos do Azure com Java | Documentos da Microsoft
description: Introdução ao envio para os Hubs de eventos com Java
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 11/12/2018
ms.author: shvija
ms.openlocfilehash: 510f1a2bc23d14e1bb9e8e561b52936ae9d53685
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624544"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Enviar eventos para Hubs de eventos do Azure com Java

Os Hubs de Eventos do Azure são uma plataforma de fluxo de Macrodados e um serviço de ingestão de eventos capaz de receber e processar milhões de eventos por segundo. Os Hubs de Eventos podem processar e armazenar eventos, dados ou telemetria produzidos por dispositivos e software distribuído. Os dados enviados para um hub de eventos podem ser transformados e armazenados em qualquer fornecedor de análise em tempo real ou adaptadores de armazenamento/criação de batches. Para uma descrição geral detalhada dos Hubs de Eventos, veja [Descrição geral dos Hubs de Eventos](event-hubs-about.md) e [Funcionalidades dos Hubs de Eventos](event-hubs-features.md).

Este tutorial mostra como enviar eventos para um hub de eventos utilizando uma aplicação de consola escrita em Java. 

> [!NOTE]
> Pode transferir este início rápido como uma amostra a partir do [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend), substituir as cadeias de carateres `EventHubConnectionString` e `EventHubName` pelos seus valores de hub de eventos e executá-la. Em alternativa, pode seguir os passos neste tutorial para criar a sua própria.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa dos seguintes pré-requisitos:

* Um ambiente de desenvolvimento do Java. Este tutorial utiliza [Eclipse](https://www.eclipse.org/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Criar um espaço de nomes de Hubs de Eventos e um hub de eventos
O primeiro passo consiste em utilizar o [portal do Azure](https://portal.azure.com) para criar um espaço de nomes do tipo Hubs de Eventos e obter as credenciais de gestão de que a sua aplicação precisa para comunicar com o hub de eventos. Para criar um espaço de nomes e um hub de eventos, siga o procedimento descrito [neste artigo](event-hubs-create.md) e, em seguida, continue com os passos seguintes deste tutorial.

## <a name="add-reference-to-azure-event-hubs-library"></a>Adicionar a referência à biblioteca dos Hubs de eventos do Azure

A biblioteca de cliente de Java dos Hubs de eventos está disponível para uso em projetos Maven a partir da [repositório Central Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Pode referenciar esta biblioteca a utilizar a seguinte declaração de dependência dentro de seu arquivo de projeto Maven. A versão atual é 1.0.2:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.2</version>
</dependency>
```

Para diferentes tipos de ambientes de compilação, pode obter explicitamente os mais recente ficheiros JAR dos [repositório Central Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Para um publicador de eventos simples, importar os *com.microsoft.azure.eventhubs* pacote para as classes de cliente dos Hubs de eventos e o *com.microsoft.azure.servicebus* como do pacote para classes de utilitários exceções comuns que são partilhadas com o cliente de mensagens do Service bus do Azure. 

## <a name="write-code-to-send-messages-to-the-event-hub"></a>Escrever códigos para enviar mensagens ao hub de eventos

Para o exemplo que se segue, comece por criar um novo projeto Maven para uma consola/aplicação shell no seu ambiente de desenvolvimento Java favorito. Nome da classe `SimpleSend`:     

```java
package com.microsoft.azure.eventhubs.samples.send;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

public class SimpleSend {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
            
            
    }
 }
```

### <a name="construct-connection-string"></a>Construir a cadeia de ligação

Utilize a classe ConnectionStringBuilder para construir um valor de cadeia de ligação para passar para a instância de cliente dos Hubs de eventos. Substitua os marcadores de posição pelos valores que obteve quando criou o hub de eventos e de espaço de nomes:

```java
final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
        .setNamespaceName("Your Event Hubs namespace name")
        .setEventHubName("Your event hub")
        .setSasKeyName("Your policy name")
        .setSasKey("Your primary SAS key");
```

### <a name="send-events"></a>Enviar eventos

Transformar uma cadeia de caracteres em seu codificação de byte de UTF-8 para criar um único evento. Em seguida, crie uma nova instância de cliente dos Hubs de eventos da cadeia de ligação e enviar a mensagem:   

```java 
String payload = "Message " + Integer.toString(i);
byte[] payloadBytes = gson.toJson(payload).getBytes(Charset.defaultCharset());
EventData sendEvent = EventData.create(payloadBytes);

final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);
ehClient.sendSync(sendEvent);
    
// close the client at the end of your program
ehClient.closeSync();

``` 

Crie e execute o programa e certifique-se de que não há nenhum erro.

Parabéns! Enviou agora mensagens para um hub de eventos.

### <a name="appendix-how-messages-are-routed-to-eventhub-partitions"></a>Apêndice: Como as mensagens são roteadas para as partições de EventHub

Antes das mensagens são obtidas pelos consumidores, têm de ser publicado para as partições primeiro pelos fabricantes. Quando as mensagens são publicadas para o hub de eventos sincronicamente usando o método de sendSync() no objeto com.microsoft.azure.eventhubs.EventHubClient, a mensagem foi enviada para uma partição específica ou distribuída para todas as partições disponíveis de forma round robin Dependendo se a chave de partição está especificada ou não.

Quando uma cadeia de caracteres que representa a chave de partição for especificada, a chave será convertida para determinar qual partição para enviar o evento para.

Quando a chave de partição não está definida, serão round robined para todas as partições disponíveis, em seguida, as mensagens

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

// close the client at the end of your program
eventHubClient.closeSync();

```

## <a name="next-steps"></a>Passos Seguintes

Neste início rápido, enviou mensagens para um hub de eventos com Java. Para saber como receber eventos de um hub de eventos com Java, veja [receber eventos do hub de eventos - Java](event-hubs-java-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

