---
title: Conector do Webhook para o Azure Logic Apps | Microsoft Docs
description: "Como utilizar as ações de webhook e acionadores para efetuar ações como filtro matriz a partir das logic apps"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: ec407d72f90cf3616214d27c378dee0ff42f4c6f
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-webhook-connector"></a>Começar a utilizar o conector do webhook

Com o acionador e ação de webhook, pode iniciar, colocar em pausa e retomar fluxos para efetuar estas tarefas:

* Acionar a partir de um [Hub de eventos do Azure](https://github.com/logicappsio/EventHubAPI) quando é recebido um item
* Aguardar a aprovação antes de continuar o fluxo de trabalho

Saiba mais sobre [como criar APIs personalizadas que suportam um webhook](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-the-webhook-trigger"></a>Utilize o acionador do webhook

A [ *acionador* ](connectors-overview.md) é um evento que inicia um fluxo de trabalho de aplicação lógica. Um acionador de webhook é baseada em eventos e não depende de consulta para novos itens. Como o [acionador pedido](connectors-native-reqres.md), a aplicação lógica desencadeado instantâneas que ocorre um evento. O acionador de webhook regista um *URL de chamada de retorno* para um serviço e utiliza esse URL a acionar a aplicação lógica conforme necessário.

Eis um exemplo que mostra como configurar um acionador HTTP no Designer de aplicação lógica. Os passos partem do princípio de que já tiver implementado ou que estão a aceder a uma API que se segue o [webhook subscrever e anular a subscrição padrão nas aplicações lógicas](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). É efetuada a chamada de subscrever sempre que uma aplicação lógica está guardada com um novo webhook ou mudou de desativado para ativado. É efetuada a chamada de anular a subscrição quando é removido e guardado ou mudou de um acionador de webhook de aplicação lógica ativado para desativado.

**Para adicionar o acionador do webhook**

1. Adicionar o **HTTP Webhook** acionador como o primeiro passo para uma aplicação lógica.
2. Preencha os parâmetros para o webhook subscrever e anular a subscrição de chamadas.

   Este passo acompanha o mesmo padrão, como o [ação HTTP](connectors-native-http.md) formato.

     ![Acionador de HTTP](./media/connectors-native-webhook/using-trigger.png)

3. Adicione pelo menos uma ação.
4. Clique em **guardar** para publicar a aplicação lógica. Este passo chama o ponto final de subscrever o URL de chamada de retorno necessários para acionar esta aplicação lógica.
5. Sempre que o serviço efetua uma `HTTP POST` para o URL de chamada de retorno, a aplicação lógica desencadeado e inclui todos os dados transmitidos para o pedido.

## <a name="use-the-webhook-action"></a>Utilize a ação do webhook

Um [ *ação* ](connectors-overview.md) é uma operação levada a cabo pelo fluxo de trabalho definido numa aplicação lógica. Uma ação do webhook regista um *URL de chamada de retorno* com um serviço e aguarda até que o URL é chamado antes de a retomar. O ["Enviar correio eletrónico aprovação"](connectors-create-api-office365-outlook.md) é um exemplo de um conector que se segue este padrão. Pode expandir este padrão em qualquer serviço através da ação do webhook. 

Eis um exemplo que mostra como configurar uma ação de webhook no Designer de aplicação lógica. Estes passos partem do princípio de que já tiver implementado ou que estão a aceder a uma API que se segue o [webhook subscrever e anular a subscrição padrão utilizada em aplicações lógicas](../logic-apps/logic-apps-create-api-app.md#webhook-actions). É efetuada a chamada de subscrever quando uma aplicação lógica executa a ação do webhook. É efetuada a chamada de anular a subscrição quando uma execução foi cancelada ao aguardar uma resposta ou, antes da lógica de aplicação exceder o tempo limite.

**Para adicionar uma ação do webhook**

1. Escolha **novo passo** > **adicionar uma ação**.

2. Na caixa de pesquisa, escreva "webhook" para localizar o **HTTP Webhook** ação.

    ![Selecione a ação de consulta](./media/connectors-native-webhook/using-action-1.png)

3. Preencha os parâmetros para o webhook subscrever e anular a subscrição de chamadas

   Este passo acompanha o mesmo padrão, como o [ação HTTP](connectors-native-http.md) formato.

     ![Ação de consulta concluída](./media/connectors-native-webhook/using-action-2.png)
   
   Em runtime, a aplicação lógica chama o ponto final de subscrever depois de atingir esse passo.

4. Clique em **guardar** para publicar a aplicação lógica.

## <a name="technical-details"></a>Detalhes técnicos

Eis mais detalhes sobre os acionadores e ações que webhook suporta.

## <a name="webhook-triggers"></a>Acionadores de Webhook

| Ação | Descrição |
| --- | --- |
| Webhook de HTTP |Subscreve um URL de chamada de retorno para um serviço que pode chamar o URL para accionar aplicação lógica, conforme necessário. |

### <a name="trigger-details"></a>Detalhes de Acionador

#### <a name="http-webhook"></a>Webhook de HTTP

Subscreve um URL de chamada de retorno para um serviço que pode chamar o URL para accionar aplicação lógica, conforme necessário.
Um * meios de campo obrigatório.

| Nome a Apresentar | Nome da propriedade | Descrição |
| --- | --- | --- |
| Subscrever o método * |método |Método de HTTP a utilizar para subscrever pedido |
| Subscrever URI * |uri |URI de HTTP a utilizar para o pedido de subscrever |
| Anular a subscrição método * |método |Método HTTP a utilizar para o pedido de anular a subscrição |
| Anular a subscrição URI * |uri |URI de HTTP a utilizar para o pedido de anular a subscrição |
| Subscrever o corpo |corpo |Corpo do pedido HTTP para subscrever |
| Subscrever cabeçalhos |cabeçalhos |Cabeçalhos de pedido HTTP para subscrever |
| Subscrever autenticação |autenticação |Autenticação HTTP a utilizar para subscrever. [Consulte o conetor HTTP](connectors-native-http.md#authentication) para obter detalhes |
| Anular a subscrição de corpo |corpo |Corpo do pedido HTTP para anular a subscrição |
| Anular a subscrição cabeçalhos |cabeçalhos |Cabeçalhos de pedido HTTP para anular a subscrição |
| Anular a subscrição de autenticação |autenticação |Autenticação HTTP a utilizar para anular a subscrição. [Consulte o conetor HTTP](connectors-native-http.md#authentication) para obter detalhes |

**Detalhes de saída**

Pedido de Webhook

| Nome da propriedade | Tipo de Dados | Descrição |
| --- | --- | --- |
| Cabeçalhos |objeto |Cabeçalhos de pedido de Webhook |
| Corpo |objeto |Objeto de pedido de Webhook |
| Código de Estado |Int |Código de estado de pedido de Webhook |

## <a name="webhook-actions"></a>Ações de Webhook

| Ação | Descrição |
| --- | --- |
| Webhook de HTTP |Subscreve um URL de chamada de retorno para um serviço que pode chamar o URL para retomar um passo de fluxo de trabalho, conforme necessário. |

### <a name="action-details"></a>Detalhes da ação

#### <a name="http-webhook"></a>Webhook de HTTP

Subscreve um URL de chamada de retorno para um serviço que pode chamar o URL para retomar um passo de fluxo de trabalho, conforme necessário.
Um * meios de campo obrigatório.

| Nome a Apresentar | Nome da propriedade | Descrição |
| --- | --- | --- |
| Subscrever o método * |método |Método de HTTP a utilizar para subscrever pedido |
| Subscrever URI * |uri |URI de HTTP a utilizar para o pedido de subscrever |
| Anular a subscrição método * |método |Método HTTP a utilizar para o pedido de anular a subscrição |
| Anular a subscrição URI * |uri |URI de HTTP a utilizar para o pedido de anular a subscrição |
| Subscrever o corpo |corpo |Corpo do pedido HTTP para subscrever |
| Subscrever cabeçalhos |cabeçalhos |Cabeçalhos de pedido HTTP para subscrever |
| Subscrever autenticação |autenticação |Autenticação HTTP a utilizar para subscrever. [Consulte o conetor HTTP](connectors-native-http.md#authentication) para obter detalhes |
| Anular a subscrição de corpo |corpo |Corpo do pedido HTTP para anular a subscrição |
| Anular a subscrição cabeçalhos |cabeçalhos |Cabeçalhos de pedido HTTP para anular a subscrição |
| Anular a subscrição de autenticação |autenticação |Autenticação HTTP a utilizar para anular a subscrição. [Consulte o conetor HTTP](connectors-native-http.md#authentication) para obter detalhes |

**Detalhes de saída**

Pedido de Webhook

| Nome da propriedade | Tipo de Dados | Descrição |
| --- | --- | --- |
| Cabeçalhos |objeto |Cabeçalhos de pedido de Webhook |
| Corpo |objeto |Objeto de pedido de Webhook |
| Código de Estado |Int |Código de estado de pedido de Webhook |

## <a name="next-steps"></a>Passos Seguintes

* [Criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Localizar outros conectores](apis-list.md)