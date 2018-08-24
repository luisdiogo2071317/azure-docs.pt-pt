---
title: Enviar eventos para Hubs de eventos do Azure com Java | Documentos da Microsoft
description: Introdução ao envio para os Hubs de eventos com Java
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 08/20/2018
ms.author: shvija
ms.openlocfilehash: 2120fedc83b1dcad5462f4d5fb5118d19c3ce91c
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/23/2018
ms.locfileid: "42747032"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Enviar eventos para Hubs de eventos do Azure com Java

Os Hubs de eventos é um sistema de ingestão altamente dimensionável, que pode ingerir milhões de eventos por segundo que uma aplicação possa processar e analisar enormes quantidades de dados produzidos pelos seus dispositivos e aplicações ligados. Depois de recolhidos para um hub de eventos, pode transformar e armazenar dados em qualquer fornecedor de análise em tempo real ou cluster de armazenamento.

Para obter mais informações, consulte a [descrição geral dos Hubs de eventos][Event Hubs overview].

Este tutorial mostra como enviar eventos para um hub de eventos utilizando uma aplicação de consola Java. Para receber eventos com a biblioteca de anfitrião do processador de eventos de Java, veja [este artigo](event-hubs-java-get-started-receive-eph.md), ou clique no idioma de receção adequado na tabela esquerda do conteúdo.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, terá dos seguintes pré-requisitos:

* Um ambiente de desenvolvimento do Java. Este tutorial utiliza [Eclipse](https://www.eclipse.org/).
* Uma conta ativa do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita][] antes de começar.

O código neste tutorial se baseia a [exemplo do SimpleSend GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend), que pode examinar para ver toda a aplicação de trabalho.

## <a name="send-events-to-event-hubs"></a>Enviar eventos para os Hubs de Eventos

A biblioteca de cliente de Java dos Hubs de eventos está disponível para uso em projetos Maven a partir da [repositório Central Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Pode referenciar esta biblioteca a utilizar a seguinte declaração de dependência dentro de seu arquivo de projeto Maven. A versão atual é 1.0.1:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.1</version>
</dependency>
```

Para diferentes tipos de ambientes de compilação, pode obter explicitamente os mais recente ficheiros JAR dos [repositório Central Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Para um publicador de eventos simples, importar os *com.microsoft.azure.eventhubs* pacote para as classes de cliente dos Hubs de eventos e o *com.microsoft.azure.servicebus* como do pacote para classes de utilitários exceções comuns que são partilhadas com o cliente de mensagens do Service bus do Azure. 

### <a name="declare-the-send-class"></a>Declarar a classe de envio

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

## <a name="next-steps"></a>Passos Seguintes

Pode saber mais sobre os Hubs de Eventos ao aceder às seguintes ligações:

* [Receber eventos com o EventProcessorHost](event-hubs-java-get-started-receive-eph.md)
* [Descrição geral dos Hubs de Eventos][Event Hubs overview]
* [Criar um hub de eventos](event-hubs-create.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[conta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

