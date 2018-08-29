---
title: Criar fluxos de trabalho baseados em eventos ou ações - Azure Logic Apps | Documentos da Microsoft
description: Automatizar fluxos de trabalho baseados em eventos ou ações através da utilização de webhooks e Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.topic: article
tags: connectors
ms.date: 07/21/2016
ms.openlocfilehash: 7b1886321ca4afd4b4710bd9fddf16d2d5eb224b
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126592"
---
# <a name="create-event-based-workflows-or-actions-by-using-webhooks-and-azure-logic-apps"></a>Criar fluxos de trabalho baseados em eventos ou ações com webhooks e Azure Logic Apps

Com o acionador e ação do webhook, pode iniciar, colocar em pausa e retomar fluxos para executar estas tarefas:

* Acionar a partir de um [Hub de eventos do Azure](https://github.com/logicappsio/EventHubAPI) quando é recebido um item
* Aguarde uma aprovação antes de continuar um fluxo de trabalho

Saiba mais sobre [como criar APIs personalizadas que suportam um webhook](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-the-webhook-trigger"></a>Utilizar o acionador de webhook

R [ *acionador* ](connectors-overview.md) é um evento que inicia um fluxo de trabalho de aplicação lógica. Um acionador de webhook é baseado em evento e não depende de sondagem para novos itens. Como o [acionador de pedido](connectors-native-reqres.md), a aplicação lógica é acionado instante que ocorre um evento. O acionador de webhook registra um *URL de retorno de chamada* para um serviço e utiliza esse URL para acionar a aplicação lógica, conforme necessário.

Eis um exemplo que mostra como configurar um acionador HTTP no Estruturador da aplicação lógica. Os passos partem do princípio de que já tiver implementado ou que está a aceder a uma API que se segue a [webhook inscrever-se e cancelar sua assinatura padrão no logic apps](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). É efetuada a chamada de inscrever-se sempre que uma aplicação lógica é guardada com um novo webhook, ou mudou de desativado para ativado. É feita a chamada de anular a subscrição quando é removido e guardado ou mudou de um acionador de webhook da aplicação de lógica ativado para desativado.

**Para adicionar o acionador de webhook**

1. Adicionar a **Webhook de HTTP** acionador, tal como o primeiro passo para uma aplicação lógica.
2. Preencha os parâmetros para o webhook inscrever-se e anular a subscrição de chamadas.

   Este passo segue o mesmo padrão como o [ação de HTTP](connectors-native-http.md) formato.

     ![Acionador HTTP](./media/connectors-native-webhook/using-trigger.png)

3. Adicione pelo menos uma ação.
4. Clique em **guardar** para publicar a aplicação lógica. Este passo chama o ponto final de inscrever-se com o URL de retorno de chamada necessário para acionar esta aplicação lógica.
5. Sempre que o serviço faz uma `HTTP POST` para o URL de retorno de chamada, a aplicação lógica é acionado e inclui todos os dados passados para o pedido.

## <a name="use-the-webhook-action"></a>Utilize a ação de webhook

Uma [ *ação* ](connectors-overview.md) é uma operação levada a cabo pelo fluxo de trabalho definido numa aplicação lógica. Uma ação de webhook regista um *URL de retorno de chamada* com um serviço e aguarda até que o URL é chamado antes de continuar. O ["Enviar E-Mail de aprovação"](connectors-create-api-office365-outlook.md) é um exemplo de um conector que segue este padrão. Pode estender esse padrão em qualquer serviço através da ação do webhook. 

Eis um exemplo que mostra como configurar uma ação de webhook no Estruturador da aplicação lógica. Estes passos partem do princípio de que já tiver implementado ou que está a aceder a uma API que se segue a [webhook inscrever-se e cancelar sua assinatura padrão usado no logic apps](../logic-apps/logic-apps-create-api-app.md#webhook-actions). A chamada de subscrever é feita quando uma aplicação lógica executa a ação do webhook. A chamada de anular a subscrição é feita quando uma execução foi cancelada ao aguardar uma resposta ou de antes da lógica de aplicação exceder o tempo limite.

**Para adicionar uma ação de webhook**

1. Escolher **novo passo** > **adicionar uma ação**.

2. Na caixa de pesquisa, escreva "webhook" para localizar os **Webhook de HTTP** ação.

    ![Selecione a ação de consulta](./media/connectors-native-webhook/using-action-1.png)

3. Preencha os parâmetros para o webhook inscrever-se e anular a subscrição de chamadas

   Este passo segue o mesmo padrão como o [ação de HTTP](connectors-native-http.md) formato.

     ![Ação de consulta completa](./media/connectors-native-webhook/using-action-2.png)
   
   No tempo de execução, a aplicação lógica chama o ponto final de inscrever-se depois de atingir esse passo.

4. Clique em **guardar** para publicar a aplicação lógica.

## <a name="technical-details"></a>Detalhes técnicos

Seguem-se mais detalhes sobre os acionadores e ações oferece suporte a esse webhook.

## <a name="webhook-triggers"></a>Acionadores de Webhook

| Ação | Descrição |
| --- | --- |
| Webhook de HTTP |Inscreva-se um URL de retorno de chamada para um serviço que pode chamar o URL para acionar a aplicação lógica, conforme necessário. |

### <a name="trigger-details"></a>Detalhes do acionador

#### <a name="http-webhook"></a>Webhook de HTTP

Inscreva-se um URL de retorno de chamada para um serviço que pode chamar o URL para acionar a aplicação lógica, conforme necessário.
Um * significa que o campo obrigatório.

| Nome a Apresentar | Nome da propriedade | Descrição |
| --- | --- | --- |
| Assine o método * |método |Método de HTTP a ser usado para o pedido de subscrever |
| Assine URI * |uri |URI de HTTP para utilizar para o pedido de subscrição |
| Anular a subscrição método * |método |Método HTTP a ser usado para o pedido de anular a subscrição |
| Anular a subscrição URI * |uri |URI de HTTP para utilizar para o pedido de anular a subscrição |
| Assine o corpo |corpo |Corpo do pedido HTTP para subscrever |
| Assine cabeçalhos |cabeçalhos |Cabeçalhos de pedido HTTP para subscrever |
| Assine autenticação |autenticação |Autenticação HTTP a utilizar para subscrever. [Consulte o conector HTTP](connectors-native-http.md#authentication) para obter detalhes |
| Anular a subscrição de corpo |corpo |Corpo do pedido HTTP para anular a subscrição |
| Anular a subscrição de cabeçalhos |cabeçalhos |Cabeçalhos de pedido HTTP para anular a subscrição |
| Anular a subscrição de autenticação |autenticação |Autenticação HTTP a utilizar para anular a subscrição. [Consulte o conector HTTP](connectors-native-http.md#authentication) para obter detalhes |

**Detalhes de saída**

Pedido de Webhook

| Nome da propriedade | Tipo de Dados | Descrição |
| --- | --- | --- |
| Cabeçalhos |objeto |Cabeçalhos de pedido de Webhook |
| Corpo |objeto |Objeto de pedido de Webhook |
| Código de Estado |Int |Código de estado do pedido de Webhook |

## <a name="webhook-actions"></a>Ações de Webhook

| Ação | Descrição |
| --- | --- |
| Webhook de HTTP |Inscreva-se um URL de retorno de chamada para um serviço que pode chamar o URL para retomar um passo de fluxo de trabalho, conforme necessário. |

### <a name="action-details"></a>Detalhes da ação

#### <a name="http-webhook"></a>Webhook de HTTP

Inscreva-se um URL de retorno de chamada para um serviço que pode chamar o URL para retomar um passo de fluxo de trabalho, conforme necessário.
Um * significa que o campo obrigatório.

| Nome a Apresentar | Nome da propriedade | Descrição |
| --- | --- | --- |
| Assine o método * |método |Método de HTTP a ser usado para o pedido de subscrever |
| Assine URI * |uri |URI de HTTP para utilizar para o pedido de subscrição |
| Anular a subscrição método * |método |Método HTTP a ser usado para o pedido de anular a subscrição |
| Anular a subscrição URI * |uri |URI de HTTP para utilizar para o pedido de anular a subscrição |
| Assine o corpo |corpo |Corpo do pedido HTTP para subscrever |
| Assine cabeçalhos |cabeçalhos |Cabeçalhos de pedido HTTP para subscrever |
| Assine autenticação |autenticação |Autenticação HTTP a utilizar para subscrever. [Consulte o conector HTTP](connectors-native-http.md#authentication) para obter detalhes |
| Anular a subscrição de corpo |corpo |Corpo do pedido HTTP para anular a subscrição |
| Anular a subscrição de cabeçalhos |cabeçalhos |Cabeçalhos de pedido HTTP para anular a subscrição |
| Anular a subscrição de autenticação |autenticação |Autenticação HTTP a utilizar para anular a subscrição. [Consulte o conector HTTP](connectors-native-http.md#authentication) para obter detalhes |

**Detalhes de saída**

Pedido de Webhook

| Nome da propriedade | Tipo de Dados | Descrição |
| --- | --- | --- |
| Cabeçalhos |objeto |Cabeçalhos de pedido de Webhook |
| Corpo |objeto |Objeto de pedido de Webhook |
| Código de Estado |Int |Código de estado do pedido de Webhook |

## <a name="next-steps"></a>Passos Seguintes

* [Criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Encontre outros conectores](apis-list.md)