---
title: Enviar eventos para os Hubs de eventos do Azure utilizando Java | Microsoft Docs
description: Começar a enviar para os Event Hubs utilizando Java
services: event-hubs
author: sethmanheim
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 05/30/2018
ms.author: sethm
ms.openlocfilehash: 6d3bf0b8ac5c5bdc7bf3deda21e800fe3cc6be2e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626416"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Enviar eventos para utilizando Java de Event Hubs do Azure

Os Event Hubs são um sistema de ingestão altamente dimensionável, que pode ingerir milhões de eventos por segundo, que uma aplicação possa processar e analisar as quantidades enormes de dados produzidos pelos seus dispositivos e aplicações ligados. Depois de recolhidos para um hub de eventos, pode transformar e armazenar dados através de qualquer fornecedor de análise em tempo real ou cluster de armazenamento.

Para obter mais informações, consulte o [descrição geral dos Event Hubs][Event Hubs overview].

Este tutorial mostra como enviar eventos para um hub de eventos utilizando uma aplicação de consola em Java. Para receber eventos utilizando a biblioteca de anfitrião do processador de eventos de Java, consulte [neste artigo](event-hubs-java-get-started-receive-eph.md), ou ao clicar no idioma adequado de receção na tabela da esquerda do conteúdo.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, terá dos seguintes pré-requisitos:

* Ambiente de desenvolvimento Java. Este tutorial utiliza [Eclipse](https://www.eclipse.org/).
* Uma conta ativa do Azure. Se não tiver uma subscrição do Azure, crie um [conta gratuita][] antes de começar.

O código neste tutorial baseia-se no [SimpleSend GitHub exemplo](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend), que pode examinar para ver o completo aplicação de trabalho.

## <a name="send-events-to-event-hubs"></a>Enviar eventos para os Event Hubs

A biblioteca de cliente de Java para os Event Hubs está disponível para utilização em projetos Maven a partir de [repositório Central Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Pode referenciar esta biblioteca utilizando a seguinte declaração de dependência dentro do ficheiro de projeto Maven. A versão atual é 1.0.0:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.0</version>
</dependency>
```

Para diferentes tipos de ambientes de compilação, pode obter explicitamente os mais recente ficheiros JAR do [repositório Central Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Para um publicador de evento simples, importar o *com.microsoft.azure.eventhubs* pacote para as classes de cliente dos Event Hubs e o *com.microsoft.azure.servicebus* , tais como o pacote para classes de utilitários exceções comuns que são partilhadas com o cliente de mensagens do Service Bus do Azure. 

### <a name="declare-the-send-class"></a>Declarar a classe de envio

Para o exemplo que se segue, comece por criar um novo projeto Maven para uma consola/aplicação shell no seu ambiente de desenvolvimento Java favorito. Designar a classe `Send`:     

```java
package com.microsoft.azure.eventhubs.samples.send;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.PartitionSender;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.Random;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

public class Send {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
```

### <a name="construct-connection-string"></a>Construir a cadeia de ligação

Utilize a classe de ConnectionStringBuilder para construir um valor de cadeia de ligação para passar para a instância de cliente dos Event Hubs. Substitua os marcadores de posição pelos valores que obteve quando criou o hub de espaço de nomes e eventos:

```java
   final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
      .setNamespaceName("----NamespaceName-----")
      .setEventHubName("----EventHubName-----")
      .setSasKeyName("-----SharedAccessSignatureKeyName-----")
      .setSasKey("---SharedAccessSignatureKey----");
```

### <a name="send-events"></a>Enviar eventos

Crie um evento único, transformar uma cadeia na respectiva codificação de bytes UTF-8. Em seguida, crie uma nova instância de cliente de Event Hubs da cadeia de ligação e envie a mensagem:   

```java 
byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
EventData sendEvent = new EventData(payloadBytes);

final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);
ehClient.sendSync(sendEvent);
    
// close the client at the end of your program
ehClient.closeSync();

``` 

## <a name="next-steps"></a>Passos Seguintes

Pode saber mais sobre os Hubs de Eventos ao aceder às seguintes ligações:

* [Receber eventos utilizando o EventProcessorHost](event-hubs-java-get-started-receive-eph.md)
* [Descrição geral dos Hubs de Eventos][Event Hubs overview]
* [Criar um hub de eventos](event-hubs-create.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[conta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

