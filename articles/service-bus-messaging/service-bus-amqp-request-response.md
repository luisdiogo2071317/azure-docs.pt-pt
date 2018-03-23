---
title: AMQP 1.0 em operações de pedido-resposta baseados do Service Bus do Azure | Microsoft Docs
description: Lista de operações baseadas em pedido/resposta do Microsoft Azure Service Bus.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: sethm
ms.openlocfilehash: d72a4de8591898a55e4225ace154fd5ed53e6f91
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>AMQP 1.0 no Microsoft Azure Service Bus: operações de pedido-resposta baseados em

Este artigo define a lista de operações baseadas em pedido/resposta do Microsoft Azure Service Bus. Esta informação é baseada no rascunho de trabalho AMQP gestão versão 1.0.  
  
Para obter um detalhadas durante a transmissão nível AMQP 1.0 protocolo guia, que explica como o Service Bus implementa e baseia-se a especificação de técnica OASIS AMQP, consulte o [AMQP 1.0 no guia de protocolo do Service Bus do Azure e do Event Hubs] [ Guia de protocolo AMQP 1.0].  
  
## <a name="concepts"></a>Conceitos  
  
### <a name="entity-description"></a>Descrição de entidade  

Uma descrição de entidade refere-se a um barramento de serviço [QueueDescription classe](/dotnet/api/microsoft.servicebus.messaging.queuedescription), [TopicDescription classe](/dotnet/api/microsoft.servicebus.messaging.topicdescription), ou [SubscriptionDescription classe](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) objeto.  
  
### <a name="brokered-message"></a>Mensagem mediada  

Representa uma mensagem no barramento de serviço, que é mapeado para uma mensagem AMQP. O mapeamento está definido no [guia de protocolo do Service Bus AMQP](service-bus-amqp-protocol-guide.md).  
  
## <a name="attach-to-entity-management-node"></a>Ligar ao nó de gestão de entidade  

Todas as operações descritas neste documento seguem um padrão de pedido/resposta, estão confinadas a uma entidade e requerem a ligação a um nó de gestão de entidade.  
  
### <a name="create-link-for-sending-requests"></a>Criar ligação para enviar pedidos  

Cria uma ligação para o nó de gestão para enviar pedidos.  
  
```  
requestLink = session.attach(     
role: SENDER,   
    target: { address: "<entity address>/$management" },   
    source: { address: ""<my request link unique address>" }   
)  
  
```  
  
### <a name="create-link-for-receiving-responses"></a>Criar ligação para a receção de respostas  

Cria uma ligação para receber as respostas a partir do nó de gestão.  
  
```  
responseLink = session.attach(    
role: RECEIVER,   
    source: { address: "<entity address>/$management" }   
    target: { address: "<my response link unique address>" }   
)  
  
```  
  
### <a name="transfer-a-request-message"></a>Transferência de uma mensagem de pedido  

Transferências de uma mensagem de pedido.  
  
```  
requestLink.sendTransfer(  
        Message(  
                properties: {  
                        message-id: <request id>,  
                        reply-to: "<my response link unique address>"  
                },  
                application-properties: {  
                        "operation" -> "<operation>",  
                },  
        )  
```  
  
### <a name="receive-a-response-message"></a>Receber uma mensagem de resposta  

Recebe a mensagem de resposta da ligação de resposta.  
  
```  
responseMessage = responseLink.receiveTransfer()  
```  
  
A mensagem de resposta é o seguinte formato:
  
```  
Message(  
properties: {     
        correlation-id: <request id>  
    },  
    application-properties: {  
            "statusCode" -> <status code>,  
            "statusDescription" -> <status description>,  
           },         
)  
  
```  
  
### <a name="service-bus-entity-address"></a>Endereço de entidade de barramento de serviço  

Entidades de barramento de serviço têm de ser resolvidas da seguinte forma:  
  
|Tipo de entidade|Endereço|Exemplo|  
|-----------------|-------------|-------------|  
|fila|`<queue_name>`|`“myQueue”`<br /><br /> `“site1/myQueue”`|  
|Tópico|`<topic_name>`|`“myTopic”`<br /><br /> `“site2/page1/myQueue”`|  
|subscrição|`<topic_name>/Subscriptions/<subscription_name>`|`“myTopic/Subscriptions/MySub”`|  
  
## <a name="message-operations"></a>Operações de mensagem  
  
### <a name="message-renew-lock"></a>Mensagem de renovação de bloqueio  

Expande o bloqueio de uma mensagem dentro do tempo especificado na descrição de entidade.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
 O corpo da mensagem de pedido de pode conter uma secção de amqp-valor que contenha um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|matriz de uuid|Sim|Tokens de bloqueio de mensagem para renovação.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – com êxito, caso contrário falha.|  
|statusDescription|string|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir exclusivamente uma secção de amqp-valor que contenha um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|Expirações|matriz de timestamp|Sim|Mensagem token novo expiração do bloqueio correspondente para os tokens de bloqueio do pedido.|  
  
### <a name="peek-message"></a>Observar mensagem  

Mensagens de peeks sem bloqueio.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|longa|Sim|Número de sequência da qual pretende iniciar peek.|  
|`message-count`|Int|Sim|Número máximo de mensagens para observar.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – tem mais mensagens<br /><br /> 0xcc: nenhum conteúdo – não existem mais mensagens|  
|statusDescription|string|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|mensagens|lista de maps|Sim|Lista de mensagens em que cada mapa representa uma mensagem.|  
  
O mapa que representa uma mensagem tem de conter as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|message|matriz de bytes|Sim|Mensagem codificado por transmissão AMQP 1.0.|  
  
### <a name="schedule-message"></a>Mensagem de agenda  

As agendas de mensagens.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|mensagens|lista de maps|Sim|Lista de mensagens em que cada mapa representa uma mensagem.|  
  
O mapa que representa uma mensagem tem de conter as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|message-id|string|Sim|`amqpMessage.Properties.MessageId` como cadeia|  
|session-id|string|Sim|`amqpMessage.Properties.GroupId as string`|  
|chave de partição|string|Sim|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|  
|message|matriz de bytes|Sim|Mensagem codificado por transmissão AMQP 1.0.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – com êxito, caso contrário falha.|  
|statusDescription|string|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir exclusivamente um **amqp valor** secção que contém um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|sequence-numbers|matriz de longa|Sim|Número de sequência de mensagens agendadas. Número de sequência é utilizado para cancelar.|  
  
### <a name="cancel-scheduled-message"></a>Cancelar a mensagem agendada  

Cancela agendada mensagens.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|sequence-numbers|matriz de longa|Sim|Números de sequência de mensagens agendadas para cancelar.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – com êxito, caso contrário falha.|  
|statusDescription|string|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir exclusivamente um **amqp valor** secção que contém um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|sequence-numbers|matriz de longa|Sim|Número de sequência de mensagens agendadas. Número de sequência é utilizado para cancelar.|  
  
## <a name="session-operations"></a>Operações de sessão  
  
### <a name="session-renew-lock"></a>Bloqueio de renovação de sessão  

Expande o bloqueio de uma mensagem dentro do tempo especificado na descrição de entidade.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|session-id|string|Sim|ID de sessão.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – tem mais mensagens<br /><br /> 0xcc: nenhum conteúdo – não existem mais mensagens|  
|statusDescription|string|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir exclusivamente um **amqp valor** secção que contém um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|expiração|carimbo de data/hora|Sim|Expiração de novo.|  
  
### <a name="peek-session-message"></a>Observar mensagem de sessão  

Mensagens de sessão de peeks sem bloqueio.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|from-sequence-number|longa|Sim|Número de sequência da qual pretende iniciar peek.|  
|message-count|Int|Sim|Número máximo de mensagens para observar.|  
|session-id|string|Sim|ID de sessão.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – tem mais mensagens<br /><br /> 0xcc: nenhum conteúdo – não existem mais mensagens|  
|statusDescription|string|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir exclusivamente um **amqp valor** secção que contém um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|mensagens|lista de maps|Sim|Lista de mensagens em que cada mapa representa uma mensagem.|  
  
 O mapa que representa uma mensagem tem de conter as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|message|matriz de bytes|Sim|Mensagem codificado por transmissão AMQP 1.0.|  
  
### <a name="set-session-state"></a>Estado da sessão de conjunto  

Define o estado de uma sessão.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|session-id|string|Sim|ID de sessão.|  
|Estado da sessão|matriz de bytes|Sim|Dados binários opaco.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – com êxito, caso contrário falha|  
|statusDescription|string|Não|Descrição do Estado.|  
  
### <a name="get-session-state"></a>Obter estado da sessão  

Obtém o estado de uma sessão.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|session-id|string|Sim|ID de sessão.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – com êxito, caso contrário falha|  
|statusDescription|string|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|Estado da sessão|matriz de bytes|Sim|Dados binários opaco.|  
  
### <a name="enumerate-sessions"></a>Enumeração de sessões  

Enumera as sessões de uma entidade de mensagens.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|tempo último atualizado|carimbo de data/hora|Sim|Filtram para incluir apenas as sessões atualizadas após num determinado momento.|  
|Ignorar|Int|Sim|Ignore um número de sessões.|  
|Parte superior|Int|Sim|Número máximo de sessões.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – tem mais mensagens<br /><br /> 0xcc: nenhum conteúdo – não existem mais mensagens|  
|statusDescription|string|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|Ignorar|Int|Sim|Número de sessões ignorados se o código de estado 200.|  
|ids de sessões|Matriz de cadeias|Sim|Matriz de IDs, se o código de estado 200 de sessão.|  
  
## <a name="rule-operations"></a>Operações de regra  
  
### <a name="add-rule"></a>Adicionar Regra  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|nome da regra|string|Sim|Nome da regra, não incluindo nomes de subscrição e o tópico.|  
|rule-description|Mapa|Sim|Descrição da regra conforme especificado na secção seguinte.|  
  
O **descrição da regra** mapa tem de incluir as seguintes entradas, onde **sql filtro** e **filtro de correlação** são mutuamente exclusivos:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|sql-filter|Mapa|Sim|`sql-filter`, conforme especificado na secção seguinte.|  
|correlation-filter|Mapa|Sim|`correlation-filter`, conforme especificado na secção seguinte.|  
|sql-rule-action|Mapa|Sim|`sql-rule-action`, conforme especificado na secção seguinte.|  
  
O mapa de filtro do sql tem de incluir as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|expressão|string|Sim|Expressão de filtro do SQL Server.|  
  
O **filtro de correlação** mapa tem de incluir, pelo menos, um dos seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|correlation-id|string|Não||  
|message-id|string|Não||  
|para|string|Não||  
|a resposta|string|Não||  
|Etiqueta|string|Não||  
|session-id|string|Não||  
|resposta-para--id de sessão|string|Não||  
|content-type|string|Não||  
|propriedades|Mapa|Não|Mapas ao Service Bus [BrokeredMessage.Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Properties).|  
  
O **ação de regra de sql** mapa tem de incluir as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|expressão|string|Sim|Expressão de ação do SQL Server.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – com êxito, caso contrário falha|  
|statusDescription|string|Não|Descrição do Estado.|  
  
### <a name="remove-rule"></a>Remover Regra  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|nome da regra|string|Sim|Nome da regra, não incluindo nomes de subscrição e o tópico.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – com êxito, caso contrário falha|  
|statusDescription|string|Não|Descrição do Estado.|  
  
## <a name="deferred-message-operations"></a>Operações de mensagem diferida  
  
### <a name="receive-by-sequence-number"></a>Receber por número de sequência  

Recebe mensagens deferidas pelo número de sequência.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:receive-by-sequence-number`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|sequence-numbers|matriz de longa|Sim|Números de sequência.|  
|receiver-settle-mode|ubyte|Sim|**Recetor liquidar os** modo conforme especificado no v 1.0 do AMQP core.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – com êxito, caso contrário falha|  
|statusDescription|string|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|mensagens|lista de maps|Sim|Lista de mensagens em que cada mapa representa uma mensagem.|  
  
O mapa que representa uma mensagem tem de conter as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|token de bloqueio|UUID|Sim|Se token bloqueio `receiver-settle-mode` é 1.|  
|message|matriz de bytes|Sim|Mensagem codificado por transmissão AMQP 1.0.|  
  
### <a name="update-disposition-status"></a>Estado de atualização de disposição  

Atualiza o estado de disposição de mensagens diferidas.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|string|Sim|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|UInt|Não|Servidor tempo limite da operação em milissegundos.|  
  
O corpo da mensagem de pedido deve consistir exclusivamente um **amqp valor** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|disposition-status|string|Sim|concluída<br /><br /> Abandonado<br /><br /> Suspenso|  
|tokens de bloqueio|matriz de uuid|Sim|Tokens de bloqueio de mensagem para atualizar o estado de disposição.|  
|deadletter-reason|string|Não|Pode ser definido se o estado de disposição está definido como **suspenso**.|  
|deadletter-description|string|Não|Pode ser definido se o estado de disposição está definido como **suspenso**.|  
|as propriedades para modificar|Mapa|Não|Lista de Service Bus mediadas para modificar as propriedades da mensagem.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – com êxito, caso contrário falha|  
|statusDescription|string|Não|Descrição do Estado.|

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre AMQP e o Service Bus, visite as seguintes ligações:

* [Descrição geral do Service Bus AMQP]
* [Guia do protocolo AMQP 1.0]
* [AMQP no barramento de serviço para o Windows Server]

[Descrição geral do Service Bus AMQP]: service-bus-amqp-overview.md
[Guia do protocolo AMQP 1.0]: service-bus-amqp-protocol-guide.md
[AMQP no barramento de serviço para o Windows Server]: https://msdn.microsoft.com/library/dn574799.asp