---
title: AMQP 1.0 em operações baseados no pedido-resposta do Service bus do Azure | Documentos da Microsoft
description: Lista de operações baseado em solicitação/resposta do Microsoft Azure Service Bus.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: spelluru
ms.openlocfilehash: f5d5b8064821dfb1aa6d4e99d0152e364f9a83fe
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700523"
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>AMQP 1.0 no Microsoft Azure Service Bus: operações baseados no pedido-resposta

Este artigo define a lista de operações baseado em solicitação/resposta do Microsoft Azure Service Bus. Estas informações se baseia o rascunho de trabalho de versão de gestão de AMQP 1.0.  
  
Para obter um detalhadas durante a transmissão nível AMQP 1.0 protocolo guia, que explica como o Service Bus implementa e baseia-se a especificação de técnica OASIS AMQP, consulte o [AMQP 1.0 no guia de protocolo do Service Bus do Azure e do Event Hubs][guia do protocolo amqp 1.0].  
  
## <a name="concepts"></a>Conceitos  
  
### <a name="entity-description"></a>Descrição da entidade  

Uma descrição da entidade refere-se como um barramento de serviço [classe QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription), [classe TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription), ou [SubscriptionDescription classe](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) objeto.  
  
### <a name="brokered-message"></a>Mensagem mediada  

Representa uma mensagem no barramento de serviço, que é mapeado para uma mensagem AMQP. O mapeamento está definido no [Guia do protocolo AMQP no Service Bus](service-bus-amqp-protocol-guide.md).  
  
## <a name="attach-to-entity-management-node"></a>Anexar ao nó de gestão de entidade  

Todas as operações descritas neste documento seguem um padrão de solicitação/resposta, estão no âmbito de uma entidade e precisam de anexação a um nó de gestão de entidade.  
  
### <a name="create-link-for-sending-requests"></a>Criar ligação para o envio de pedidos  

Cria uma ligação para o nó de gestão para o envio de pedidos.  
  
```  
requestLink = session.attach(     
role: SENDER,   
    target: { address: "<entity address>/$management" },   
    source: { address: ""<my request link unique address>" }   
)  
  
```  
  
### <a name="create-link-for-receiving-responses"></a>Criar ligação para a receber respostas  

Cria uma ligação para receber as respostas do nó de gestão.  
  
```  
responseLink = session.attach(    
role: RECEIVER,   
    source: { address: "<entity address>/$management" }   
    target: { address: "<my response link unique address>" }   
)  
  
```  
  
### <a name="transfer-a-request-message"></a>Transferência de uma mensagem de pedido  

Transfere uma mensagem de pedido.  
Um Estado da transação pode ser adicionado, opcionalmente, para operações que suporta a transação.

```  
requestLink.sendTransfer(  
        Message(  
                properties: {  
                        message-id: <request id>,  
                        reply-to: "<my response link unique address>"  
                },  
                application-properties: {  
                        "operation" -> "<operation>",  
                }
        ),
        [Optional] State = transactional-state: {
                txn-id: <txn-id>
        }
)
```  
  
### <a name="receive-a-response-message"></a>Receber uma mensagem de resposta  

Recebe a mensagem de resposta a partir da ligação de resposta.  
  
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
  
### <a name="service-bus-entity-address"></a>Endereço de entidade do Service Bus  

Entidades do Service Bus devem ser resolvidas da seguinte forma:  
  
|Tipo de entidade|Endereço|Exemplo|  
|-----------------|-------------|-------------|  
|fila|`<queue_name>`|`“myQueue”`<br /><br /> `“site1/myQueue”`|  
|tópico|`<topic_name>`|`“myTopic”`<br /><br /> `“site2/page1/myQueue”`|  
|subscrição|`<topic_name>/Subscriptions/<subscription_name>`|`“myTopic/Subscriptions/MySub”`|  
  
## <a name="message-operations"></a>Operações de mensagem  
  
### <a name="message-renew-lock"></a>Mensagem de renovar bloqueio  

Estende o bloqueio de uma mensagem, o período de tempo especificado na descrição de entidade.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
 O corpo da mensagem de pedido deve ser composto por uma seção de amqp-valor que contém um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|matriz de uuid|Sim|Tokens de bloqueio da mensagem para renovar.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – êxito, caso contrário falha.|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve ser composto por uma seção de amqp-valor que contém um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|Expirações|matriz de timestamp|Sim|Mensagem bloqueio novo expiração do token correspondente para os tokens de bloqueio do pedido.|  
  
### <a name="peek-message"></a>Observar mensagem  

Lê as mensagens sem bloqueio.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|longitude|Sim|Número de sequência da qual pretende iniciar a pré-visualização.|  
|`message-count`|Int|Sim|Número máximo de mensagens de olhar.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – tem mais mensagens<br /><br /> 0xcc: não conteúdo – não existem mais mensagens|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir numa **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|mensagens|lista de mapas|Sim|Lista de mensagens em que cada mapa representa uma mensagem.|  
  
O mapa que representa uma mensagem tem de conter as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|message|matriz de bytes|Sim|Mensagem codificada durante a transmissão do AMQP 1.0.|  
  
### <a name="schedule-message"></a>Mensagem de agenda  

As agendas de mensagens. Esta operação podporuje transakci.
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|mensagens|lista de mapas|Sim|Lista de mensagens em que cada mapa representa uma mensagem.|  
  
O mapa que representa uma mensagem tem de conter as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|id da mensagem|cadeia|Sim|`amqpMessage.Properties.MessageId` como cadeia de caracteres|  
|id de sessão|cadeia|Não|`amqpMessage.Properties.GroupId as string`|  
|chave de partição|cadeia|Não|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|
|por meio de-chave de partição|cadeia|Não|`amqpMessage.MessageAnnotations."x-opt-via-partition-key"`|
|message|matriz de bytes|Sim|Mensagem codificada durante a transmissão do AMQP 1.0.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – êxito, caso contrário falha.|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir numa **amqp valor** secção que contém um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|números de sequência|matriz de longa duração|Sim|Número de sequência de mensagens agendadas. Número de sequência é utilizado para cancelar.|  
  
### <a name="cancel-scheduled-message"></a>Cancelar mensagem agendada  

Cancela a agendar mensagens.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|números de sequência|matriz de longa duração|Sim|Números de sequência de mensagens agendadas para cancelar.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – êxito, caso contrário falha.|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir numa **amqp valor** secção que contém um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|números de sequência|matriz de longa duração|Sim|Número de sequência de mensagens agendadas. Número de sequência é utilizado para cancelar.|  
  
## <a name="session-operations"></a>Operações de sessão  
  
### <a name="session-renew-lock"></a>Sessão renovar bloqueio  

Estende o bloqueio de uma mensagem, o período de tempo especificado na descrição de entidade.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|id de sessão|cadeia|Sim|ID de sessão.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – tem mais mensagens<br /><br /> 0xcc: não conteúdo – não existem mais mensagens|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir numa **amqp valor** secção que contém um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|expiração|carimbo de data/hora|Sim|Expiração de novo.|  
  
### <a name="peek-session-message"></a>Observar mensagem de sessão  

Lê as mensagens de sessão sem bloqueio.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|from-sequence-number|longitude|Sim|Número de sequência da qual pretende iniciar a pré-visualização.|  
|Contagem de mensagens|Int|Sim|Número máximo de mensagens de olhar.|  
|id de sessão|cadeia|Sim|ID de sessão.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – tem mais mensagens<br /><br /> 0xcc: não conteúdo – não existem mais mensagens|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir numa **amqp valor** secção que contém um mapa com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|mensagens|lista de mapas|Sim|Lista de mensagens em que cada mapa representa uma mensagem.|  
  
 O mapa que representa uma mensagem tem de conter as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|message|matriz de bytes|Sim|Mensagem codificada durante a transmissão do AMQP 1.0.|  
  
### <a name="set-session-state"></a>Estado da sessão de conjunto  

Define o estado de uma sessão.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|id de sessão|cadeia|Sim|ID de sessão.|  
|Estado da sessão|matriz de bytes|Sim|Dados binários opacos.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – êxito, caso contrário falha|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
### <a name="get-session-state"></a>Estado da sessão de GET  

Obtém o estado de uma sessão.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|id de sessão|cadeia|Sim|ID de sessão.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – êxito, caso contrário falha|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir numa **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|Estado da sessão|matriz de bytes|Sim|Dados binários opacos.|  
  
### <a name="enumerate-sessions"></a>Enumerar sessões  

Enumera as sessões numa entidade de mensagens.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|última-atualizado-time|carimbo de data/hora|Sim|Filtre para incluir apenas as sessões atualizadas após um determinado momento.|  
|ignorar|Int|Sim|Ignore um número de sessões.|  
|parte superior|Int|Sim|Número máximo de sessões.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – tem mais mensagens<br /><br /> 0xcc: não conteúdo – não existem mais mensagens|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir numa **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|ignorar|Int|Sim|Número de sessões ignoradas se o código de estado é 200.|  
|ids de sessões|matriz de cadeias de caracteres|Sim|Matriz de IDs, se o código de estado é 200 de sessão.|  
  
## <a name="rule-operations"></a>Operações de regra  
  
### <a name="add-rule"></a>Adicionar Regra  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|nome da regra|cadeia|Sim|Nome da regra, não incluindo nomes de subscrição e tópico.|  
|Descrição da regra|Mapa|Sim|Descrição da regra conforme especificado na secção seguinte.|  
  
O **descrição da regra** mapa tem de incluir as seguintes entradas, onde **filtro de sql** e **filtro de correlação** são mutuamente exclusivas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|sql-filter|Mapa|Sim|`sql-filter`, conforme especificado na secção seguinte.|  
|filtro de correlação|Mapa|Sim|`correlation-filter`, conforme especificado na secção seguinte.|  
|sql-rule-action|Mapa|Sim|`sql-rule-action`, conforme especificado na secção seguinte.|  
  
O mapa de filtro do sql tem de incluir as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|expressão|cadeia|Sim|Expressão de filtro de SQL.|  
  
O **filtro de correlação** mapa tem de incluir, pelo menos, uma das seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|id de correlação|cadeia|Não||  
|id da mensagem|cadeia|Não||  
|para|cadeia|Não||  
|responder a|cadeia|Não||  
|label|cadeia|Não||  
|id de sessão|cadeia|Não||  
|resposta-para--id de sessão|cadeia|Não||  
|tipo de conteúdo|cadeia|Não||  
|propriedades|Mapa|Não|É mapeado para o Service Bus [BrokeredMessage.Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Properties).|  
  
O **ação de regra sql** mapa tem de incluir as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|expressão|cadeia|Sim|Expressão de ação de SQL.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – êxito, caso contrário falha|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
### <a name="remove-rule"></a>Remover Regra  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|nome da regra|cadeia|Sim|Nome da regra, não incluindo nomes de subscrição e tópico.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – êxito, caso contrário falha|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
### <a name="get-rules"></a>Obter regras

#### <a name="request"></a>Pedir

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:

|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:enumerate-rules`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  

O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|parte superior|Int|Sim|O número de regras para buscar na página.|  
|ignorar|Int|Sim|O número de regras para ignorar. Define o índice de início (+ 1) na lista de regras. | 

#### <a name="response"></a>Resposta

A mensagem de resposta inclui as seguintes propriedades:

|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – êxito, caso contrário falha|  
|regras| matriz de mapa|Sim|Matriz de regras. Cada regra é representada por um mapa.|

Cada entrada de mapa na matriz inclui as seguintes propriedades:

|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|Descrição da regra|matriz de objetos descritos|Sim|`com.microsoft:rule-description:list` com o AMQP descrito código 0x0000013700000004| 

`com.microsoft.rule-description:list` é uma matriz de objetos descritos. A matriz inclui o seguinte:

|Índice|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
| 0 | matriz de objetos descritos | Sim | `filter` conforme especificado abaixo. |
| 1 | matriz de objeto descrito | Sim | `ruleAction` conforme especificado abaixo. |
| 2 | cadeia | Sim | nome da regra. |

`filter` pode ser de qualquer um dos seguintes tipos:

| Nome do descritor | Código de descritor | Valor |
| --- | --- | ---|
| `com.microsoft:sql-filter:list` | 0x000001370000006 | Filtro SQL |
| `com.microsoft:correlation-filter:list` | 0x000001370000009 | Filtro de correlação |
| `com.microsoft:true-filter:list` | 0x000001370000007 | VERDADEIRO filtro que representa 1 = 1 |
| `com.microsoft:false-filter:list` | 0x000001370000008 | FALSO filtro que representa 1 = 0 |

`com.microsoft:sql-filter:list` é uma matriz descrita, que inclui:

|Índice|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
| 0 | cadeia | Sim | Expressão de filtro de SQL |

`com.microsoft:correlation-filter:list` é uma matriz descrita, que inclui:

|Índice (se existir)|Tipo de valor|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
| 0 | cadeia | ID de Correlação |
| 1 | cadeia | ID da mensagem |
| 2 | cadeia | Para |
| 3 | cadeia | Responder A |
| 4 | cadeia | Etiqueta |
| 5 | cadeia | ID de sessão |
| 6 | cadeia | Responder a ID da Sessão|
| 7 | cadeia | Tipo de Conteúdo |
| 8 | Mapa | Mapa da aplicação definir propriedades |

`ruleAction` pode ser qualquer um dos seguintes tipos:

| Nome do descritor | Código de descritor | Valor |
| --- | --- | ---|
| `com.microsoft:empty-rule-action:list` | 0x0000013700000005 | Ação de regra vazia - nenhuma ação de regra presente |
| `com.microsoft:sql-rule-action:list` | 0x0000013700000006 | Ação de regra SQL |

`com.microsoft:sql-rule-action:list` é uma matriz de objetos descritos cuja primeira entrada é uma cadeia de caracteres que contém a expressão da ação de regra SQL.

## <a name="deferred-message-operations"></a>Operações de mensagem diferida  
  
### <a name="receive-by-sequence-number"></a>Receber pelo número de sequência  

Recebe mensagens diferidas pelo número de sequência.  
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:receive-by-sequence-number`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|números de sequência|matriz de longa duração|Sim|Números de sequência.|  
|receiver-settle-mode|ubyte|Sim|**Liquidação de recetor** modo conforme especificado na versão 1.0 de núcleo AMQP.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – êxito, caso contrário falha|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|  
  
O corpo da mensagem de resposta deve consistir numa **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|mensagens|lista de mapas|Sim|Lista de mensagens em que cada mapa representa uma mensagem.|  
  
O mapa que representa uma mensagem tem de conter as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|token de bloqueio|UUID|Sim|If de token de bloqueio `receiver-settle-mode` é 1.|  
|message|matriz de bytes|Sim|Mensagem codificada durante a transmissão do AMQP 1.0.|  
  
### <a name="update-disposition-status"></a>Estado de atualização de disposição  

Atualiza o estado de disposição de mensagens diferidas. Esta operação oferece suporte a transações.
  
#### <a name="request"></a>Pedir  

A mensagem de pedido tem de incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|operação|cadeia|Sim|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|UInt|Não|Operação tempo limite do servidor em milissegundos.|  
  
O corpo da mensagem de pedido deve ser composto um **valor de amqp** secção que contém um **mapa** com as seguintes entradas:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|Estado de disposição|cadeia|Sim|completed<br /><br /> abandonada<br /><br /> Suspenso|  
|tokens de bloqueio|matriz de uuid|Sim|Tokens de bloqueio da mensagem para atualizar o estado de disposição.|  
|deadletter-reason|cadeia|Não|Pode ser definido se o estado de disposição está definido como **suspenso**.|  
|deadletter-description|cadeia|Não|Pode ser definido se o estado de disposição está definido como **suspenso**.|  
|propriedades para modificar|Mapa|Não|Lista de Service Bus mediadas para modificar as propriedades da mensagem.|  
  
#### <a name="response"></a>Resposta  

A mensagem de resposta deve incluir as seguintes propriedades da aplicação:  
  
|Chave|Tipo de valor|Necessário|Conteúdo de valor|  
|---------|----------------|--------------|--------------------|  
|statusCode|Int|Sim|Código de resposta HTTP [RFC2616]<br /><br /> 200: OK – êxito, caso contrário falha|  
|Descriçãoestado|cadeia|Não|Descrição do Estado.|

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o AMQP e o Service Bus, visite as seguintes ligações:

* [Descrição geral do AMQP no Service Bus]
* [Guia do protocolo AMQP 1.0]
* [AMQP no Service Bus para o Windows Server]

[Descrição geral do AMQP no Service Bus]: service-bus-amqp-overview.md
[Guia do protocolo AMQP 1.0]: service-bus-amqp-protocol-guide.md
[AMQP no Service Bus para o Windows Server]: https://docs.microsoft.com/previous-versions/service-bus-archive/dn282144(v=azure.100)