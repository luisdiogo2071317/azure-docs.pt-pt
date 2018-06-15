---
title: Como registar eventos aos Hubs de eventos do Azure na API Management do Azure | Microsoft Docs
description: Saiba como registar eventos aos Hubs de eventos do Azure na API Management do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: apimpm
ms.openlocfilehash: 3f4da70d94d28496f5b08035ead0ef7acf1ca3bc
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
ms.locfileid: "29969602"
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Como registar eventos aos Hubs de eventos do Azure na API Management do Azure
Os Event Hubs do Azure são um serviço de entrada de dados altamente dimensionável, que pode ingerir milhões de eventos por segundo para que possa processar e analisar os quantidades enormes de dados produzidos pelos dispositivos e aplicações ligados. Os Event Hubs atuam como a "porta da frente" para um pipeline de eventos e, depois dos dados são recolhidos para um hub de eventos, podem ser transformado e armazenados através de qualquer fornecedor de análise em tempo real ou adaptadores de criação de batches/armazenamento. Os Event Hubs desacoplam a produção de um fluxo de eventos do consumo desses eventos, para que os consumidores de eventos possam aceder aos eventos de acordo com seu próprio agendamento.

Este artigo é um complemento para a [integrar a gestão de API do Azure com os Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) vídeo e descreve como registar eventos de API Management com o Event Hubs do Azure.

## <a name="create-an-azure-event-hub"></a>Criar um Hub de eventos do Azure

Para obter passos detalhados sobre como criar um hub de eventos e obter as cadeias de ligação terá de enviar e receber eventos de e para o Hub de eventos, consulte [criar um espaço de nomes de Event Hubs e um hub de eventos no portal do Azure](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="create-an-api-management-logger"></a>Criar um registo de API Management
Agora que tem um Hub de eventos, o passo seguinte consiste em configurar um [registador](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) na gestão de API do serviço para que possa registar eventos para o Hub de eventos.

Registadores de API Management são configurados utilizando o [API de REST de gestão de API](http://aka.ms/smapi). Antes de utilizar a API REST pela primeira vez, reveja o [pré-requisitos](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) e certifique-se de que tem [ativou o acesso à REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).

Para criar um registo, efetue um pedido HTTP PUT utilizando o modelo de URL seguinte:

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2017-03-01`

* Substitua `{your service}` com o nome da sua instância do serviço de API Management.
* Substitua `{new logger name}` com o nome pretendido para o novo registo. Referência a este nome ao configurar o [registo-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) política

Adicione os seguintes cabeçalhos para o pedido:

* Tipo de conteúdo: application/json
* Autorização: SharedAccessSignature 58...
  * Para obter instruções sobre a gerar o `SharedAccessSignature` consulte [autenticação de API do REST de gestão do Azure API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).

Especifique o corpo do pedido utilizando o modelo seguinte:

```json
{
  "loggerType" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of the Event Hub from the Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* `loggerType` tem de ser definido como `AzureEventHub`.
* `description` Fornece uma descrição opcional do registo e pode ser uma cadeia de comprimento zero se assim o desejar.
* `credentials` contém o `name` e `connectionString` do seu Hub de eventos do Azure.

Quando efetua o pedido, se o registo é criado um código de estado de `201 Created` é devolvido.

> [!NOTE]
> Para outros códigos de retorno possíveis e os respetivos motivos, consulte [criar um registo](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT). Para ver como realizar outras operações, tais como a lista, update e delete, consulte o [registador](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) documentação de entidade.
>
>

## <a name="configure-log-to-eventhubs-policies"></a>Configurar políticas de registo-eventhubs

Depois de ter configurado o registo na API Management, pode configurar as políticas de registo-eventhubs para registar eventos de pretendido. A política de registo-eventhubs pode ser utilizada na secção de entrada de política ou a secção de política de saída.

1. Navegue até à sua instância APIM.
2. Selecione o separador de API.
3. Selecione a API para o qual pretende adicionar a política. Neste exemplo, estamos a adicionar uma política para o **API eco** no **ilimitada** produto.
4. Selecione **todas as operações**.
5. Na parte superior do ecrã, selecione o separador Estrutura.
6. Na janela de processamento de entrada ou de saída, clique em triângulo (junto a lápis).
7. Selecione o editor de código. Para obter mais informações, consulte [como definir ou editar as políticas](set-edit-policies.md).
8. Posicione o cursor no `inbound` ou `outbound` secção política.
9. Na janela no lado direito, selecione **políticas avançadas** > **registo a EventHub**. Isto insere a `log-to-eventhub` modelo de política de instrução.

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```
Substitua `logger-id` com o nome do registo de API Management que configurou no passo anterior.

Pode utilizar uma expressão que devolve uma cadeia como o valor para o `log-to-eventhub` elemento. Neste exemplo, uma cadeia contendo a data e hora, nome do serviço, id do pedido, endereço de ip do pedido e o nome da operação é registada.

Clique em **guardar** para guardar a configuração de política atualizado. Assim que é guardado a política estar ativa e os eventos são registados para o Hub de eventos designado.

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre os Event Hubs do Azure
  * [Introdução ao Event Hubs do Azure](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Receber mensagens com o EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Guia de programação dos Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Saiba mais sobre a integração de API Management e do Event Hubs
  * [Referência de entidade de registo](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity)
  * [referência de política de registo para eventhub](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Monitorizar as suas APIs com API Management do Azure, os Event Hubs e Runscope](api-management-log-to-eventhub-sample.md)    

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
