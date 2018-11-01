---
title: Domínios de evento na grelha de eventos do Azure
description: Descreve como os domínios de eventos são utilizados para gerir os tópicos do Azure Event Grid.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: fe66ca8b8f5b4474290e302f73b35868dce68caa
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634321"
---
# <a name="understand-event-domains-for-managing-event-grid-topics"></a>Compreender os domínios de eventos para o gerenciamento de tópicos do Event Grid

Este artigo descreve como utilizar domínios de eventos para gerenciar o fluxo de eventos personalizados para várias organizações do negócio, os clientes ou aplicações. Domínios de eventos de utilização para:

* Faça a gestão de arquiteturas de multi-inquilino de eventos à escala.
* Gerir a sua autorização e autenticação.
* Particione os tópicos sem ter de gerir cada um individualmente.
* Evite publicar individualmente para cada um dos seus pontos de extremidade do tópico.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]


## <a name="event-domains-overview"></a>Descrição geral de domínios de eventos

Um domínio de eventos é uma ferramenta de gestão para um grande número de tópicos do Event Grid relacionadas com a mesma aplicação. Pode pensá-lo como um tópico meta que pode conter a milhares de tópicos individuais.

Domínios de evento tornam a arquitetura de serviços do Azure, como a utilizam o armazenamento e o IoT Hub para publicar seus eventos disponíveis para utilização no seu próprio sistema. Eles permitem que publicar eventos para milhares de tópicos. Domínios também dão-lhe controlo de autorização e autenticação sobre cada tópico para que pode dividir os seus inquilinos.

### <a name="example-use-case"></a>Caso de utilização de exemplo

Domínios de eventos mais facilmente são explicados com um exemplo. Digamos que executar o mecanismo de construção da Contoso, em que fabrica tractors, compreendendo o equipamento e outra maquinaria pesada. Como parte da execução da empresa, enviar informações em tempo real para os clientes em relação à manutenção de equipamento, estado de funcionamento de sistemas, de contrato de atualizações, etc. Todas essas informações vai para vários pontos de extremidade incluindo a sua aplicação, os pontos de extremidade do cliente e outros clientes de infraestrutura têm o programa de configuração.

Domínios de eventos permite-lhe ao modelo de mecanismo de construção da Contoso como uma entidade de eventos único. Cada um dos seus clientes é representada como um tópico dentro do domínio / autenticação e autorização são processadas com o Azure Active Directory. Cada um dos seus clientes pode subscrever o tópico e obter os seus eventos fornecidos às mesmas. Acesso gerido através do domínio de eventos garante que só possam aceder ao respetivo tópico.

Ele também fornece um único ponto de extremidade, o que poderá publicar todos os eventos de cliente para. Grelha de eventos se encarregará de certificar-se de que cada tópico apenas tem conhecimento de eventos no âmbito do seu inquilino.

![Exemplo de construção de contoso](./media/event-domains/contoso-construction-example.png)

## <a name="access-management"></a>Gestão de acesso

 Com um domínio, tem de controlo de autorização e autenticação de refinado sobre cada tópico através baseado do Azure em funções acesso verificar (RBAC). Pode utilizar estas funções para restringir a cada inquilino na sua aplicação para apenas os tópicos que pretende conceder-lhes acesso a.

Função de acesso baseado em verificar (RBAC) em domínios de evento funciona da mesma maneira [geridos pelo controlo de acesso](https://docs.microsoft.com/azure/event-grid/security-authentication#management-access-control) funciona no resto do Event Grid e o Azure. Utilize o RBAC para criar e impor definições de funções personalizadas em domínios de evento.

### <a name="built-in-roles"></a>Funções incorporadas

Grelha de eventos tem duas definições de função incorporada para facilitar o RBAC:

#### <a name="eventgrid-eventsubscription-contributor-preview"></a>Contribuinte de EventSubscription EventGrid (pré-visualização)

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

#### <a name="eventgrid-eventsubscription-reader-preview"></a>Leitor de EventSubscription EventGrid (pré-visualização)

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

## <a name="subscribing-to-topics"></a>Subscrição de tópicos

Subscrever a eventos num tópico dentro de um domínio de evento é igual a [criar uma subscrição de evento num tópico personalizado](./custom-event-quickstart.md) ou subscrever a qualquer um dos incorporada ofertas do Azure de publicadores de eventos.

### <a name="domain-scope-subscriptions"></a>Subscrições de âmbito de domínio

Domínios de evento também permitem subscrições de âmbito de domínio. Uma subscrição de evento num domínio de eventos irá receber todos os eventos enviados para o domínio, independentemente do tópico que os eventos são enviados para. Subscrições de âmbito de domínio podem ser úteis para gestão e fins de auditoria.

## <a name="publishing-to-an-event-domain"></a>Publicação de um domínio de eventos

Quando cria um domínio de evento, é-lhe fornecido um ponto final de publicação semelhante se tivesse criado um tópico no Event Grid. 

Para publicar eventos para nenhum tópico num domínio de evento, enviar os eventos para o ponto de extremidade do domínio a [mesma forma que faria para um tópico personalizado](./post-to-custom-topic.md). A única diferença é que tem de especificar o tópico que gostaria de ter o evento seja entregue ao.

Por exemplo, a matriz a seguir de eventos de publicação seria enviar eventos com `"id": "1111"` para o tópico `foo` enquanto o evento com `"id": "2222"` seriam enviados para o tópico `bar`:

```json
[{
  "topic": "foo",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "bar",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Domínios de evento lidar com a publicação para tópicos para. Em vez de publicar eventos para cada tópico que gerir individualmente, pode publicar todos os eventos para o ponto de extremidade do domínio e Event Grid encarrega-se de garantir que cada evento é enviado para o tópico correto.

## <a name="limits-and-quotas"></a>Limites e quotas

### <a name="control-plane"></a>Plano de controlo

Durante a pré-visualização, os domínios de evento serão limitados a 1000 tópicos dentro de um domínio e de 50 subscrições de eventos por tópico dentro de um domínio. Subscrições de âmbito de domínio de eventos também será limitadas a 50.

### <a name="data-plane"></a>Plano de dados

Durante a pré-visualização, o débito de eventos para um domínio de eventos será limitado para os mesmos 5.000 eventos por segundo velocidade de ingestão que tópicos personalizados estão limitados a.

## <a name="pricing"></a>Preços

Durante a pré-visualização, os domínios de eventos irá utilizar o mesmo [preços de operações](https://azure.microsoft.com/pricing/details/event-grid/) que utilizam todos os outros recursos no Event Grid.

Operações funcionam da mesma em domínios de evento tal como nos tópicos personalizados. Cada entrada de um evento a um domínio de eventos é uma operação e cada tentativa de entrega de um evento é uma operação.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre como configurar domínios de eventos, criar tópicos, subscrições de eventos de criação e publicação de eventos, consulte [gerir domínios de evento](./how-to-event-domains.md).