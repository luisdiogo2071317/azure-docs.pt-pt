---
title: Descrição geral do Azure Event Grid
description: Descreve o Azure Event Grid e respetivos conceitos.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: overview
ms.date: 04/27/2018
ms.author: babanisa
ms.openlocfilehash: f1d235fe431cfe14019ffef7c043dfbc367bb2bc
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/18/2018
ms.locfileid: "34303982"
---
# <a name="an-introduction-to-azure-event-grid"></a>Introdução ao Azure Event Grid

O Azure Event Grid permite-lhe criar facilmente aplicações com arquiteturas baseadas em eventos. Tem de selecionar o recurso do Azure que pretende subscrever e indicar o processador de eventos ou o ponto final do webHook para o qual enviar o evento. O Event Grid tem suporte incorporado para eventos provenientes de serviços do Azure, como os blobs de armazenamento e os grupos de recursos. Também tem suporte personalizado para eventos de aplicações e eventos de terceiros, mediante a utilização de tópicos personalizados e webhooks personalizados. 

Pode utilizar filtros para encaminhar eventos específicos para diferentes pontos finais, entregar em vários pontos finais e confirmar que os eventos são entregues com confiança. O Event Grid tem igualmente suporte incorporado para eventos personalizados e de terceiros.

Atualmente, o Event Grid suporta as seguintes regiões:

* Sudeste Asiático
* Ásia Oriental
* Leste da Austrália
* Sudeste da Austrália
* EUA Central
*   EUA Leste
*   EUA Leste 2
* Europa Ocidental
* Norte da Europa
* Leste do Japão
* Oeste do Japão
*   EUA Centro-Oeste
*   EUA Oeste
*   EUA Oeste 2

Este artigo mostra uma descrição geral do Azure Event Grid. Se quiser começar a utilizar o Event Grid, veja [Criar e encaminhar eventos personalizados com o Azure Event Grid](custom-event-quickstart.md). A imagem seguinte mostra como de que forma é que o Event Grid liga origens e processadores, mas não disponibiliza uma lista abrangente das opções suportadas.

![Modelo funcional do Event Grid](./media/overview/functional-model.png)

## <a name="event-sources"></a>Origens de eventos

Atualmente, os seguintes serviços do Azure suportam o envio de eventos para o Event Grid:

* Subscrições do Azure (operações de gestão)
* Tópicos Personalizados
* Event Hubs
* IoT Hub
* Serviços de Multimédia
* Grupos de Recursos (operações de gestão)
* Service Bus
* Blob de Armazenamento
* V2 de Fins Gerais de Armazenamento (GPv2)

Para obter ligações para artigos que mostram como utilizar cada origem de eventos, veja [Event sources in Azure Event Grid](event-sources.md) (Origens de eventos no Azure Event Grid).

## <a name="event-handlers"></a>Processadores de eventos

Atualmente, os seguintes serviços do Azure suportam o processamento de eventos no Event Grid: 

* Automatização do Azure
* Funções do Azure
* Event Hubs
* Ligações Híbridas
* Aplicações Lógicas
* Microsoft Flow
* Armazenamento de filas
* WebHooks

Se utilizar as Funções do Azure como o processador, utilize o acionador do Event Grid em vez dos acionadores HTTP genéricos. O Event Grid valida automaticamente os acionadores de função do Event Grid. Com os acionadores HTTP genéricos, tem de implementar a [resposta de validação](security-authentication.md#webhook-event-delivery).

Para obter ligações para artigos que mostram como utilizar cada processador de eventos, veja [Event handlers in Azure Event Grid](event-handlers.md) (Processadores de eventos no Azure Event Grid).

## <a name="concepts"></a>Conceitos

Existem cinco conceitos no Azure Event Grid que lhe permitem começar:

* **Eventos** - o que aconteceu.
* **Origens/publicadores de eventos** - onde o evento aconteceu.
* **Tópicos** - o ponto final no qual os publicadores enviam eventos.
* **Subscrições de eventos** - o ponto final ou o mecanismo incorporado para encaminhar eventos, por vezes, para vários processadores. As subscrições também são utilizadas pelos processadores para filtrar inteligentemente os eventos recebidos.
* **Processadores de eventos** - a aplicação ou o serviço que reage ao evento.

Para obter mais informações sobre estes conceitos, veja [Concepts in Azure Event Grid](concepts.md) (Conceitos no Azure Event Grid).

## <a name="capabilities"></a>Capacidades

Seguem-se algumas das principais funcionalidades do Azure Event Grid:

* **Simplicidade** - aponte e clique para direcionar eventos do seu recurso do Azure para qualquer processador ou ponto final de eventos.
* **Filtragem avançada** - filtre por tipo de evento ou por caminho de publicador do evento para garantir que os processadores de eventos só recebem eventos relevantes.
* **Fan-out** - subscreva vários pontos finais para o mesmo evento, para enviar cópias do mesmo para tantos locais quanto necessário.
* **Fiabilidade** - utilize a repetição de 24 horas com término exponencial para garantir que os eventos são entregues.
* **Pagar por evento** - pague apenas pela quantidade utilizada no Event Grid.
* **Débito elevado** - crie cargas de trabalho de elevado volume no Event Grid com suporte para milhões de eventos por segundo.
* **Eventos incorporados** - comece a executar rapidamente com os eventos incorporados definidos por recursos.
* **Eventos personalizados** - utilize as rotas e o filtro do Event Grid para entregar, com confiança, eventos personalizados à sua aplicação.

Para ver uma comparação entre o Event Grid, os Hubs de Eventos e o Service Bus, veja [Choose between Azure services that deliver messages](compare-messaging-services.md) (Escolher entre serviços do Azure que entregam mensagens).

## <a name="what-can-i-do-with-event-grid"></a>O que posso fazer com o Event Grid?

O Azure Event Grid oferece múltiplas capacidades que melhoram largamente o trabalho sem servidores, a automatização de operações e o trabalho de integração: 

### <a name="serverless-application-architectures"></a>Arquiteturas de aplicações sem servidor

![Aplicação sem servidor](./media/overview/serverless_web_app.png)

O Event Grid liga as origens de dados e os processadores de eventos. Por exemplo, utilize o Event Grid para acionar instantaneamente uma função sem servidor para executar imagens de análises sempre que é adicionada uma fotografia nova a um contentor de armazenamento de blobs. 

### <a name="ops-automation"></a>Automatização de operações

![Automatização de operações](./media/overview/Ops_automation.png)

O Event Grid permite-lhe acelerar a automatização e simplificar a aplicação de políticas. Por exemplo, o Event Grid pode notificar a Automatização do Azure quando é criada uma máquina virtual ou quando é preparada uma Base de Dados SQL. Estes eventos podem ser utilizados para verificar automaticamente se as configurações do serviço estão em conformidade, colocar metadados em ferramentas de operações, etiquetar máquinas virtuais ou arquivar itens de trabalho.

### <a name="application-integration"></a>Integração de aplicações

![Integração de aplicações](./media/overview/app_integration.png)

O Event Grid liga a sua aplicação a outros serviços. Por exemplo, crie um tópico personalizado para enviar os dados de eventos da aplicação para o Event Grid e tire partido da entrega fiável, do encaminhamento avançado e da integração direta no Azure deste serviços. Em alternativa, pode utilizar o Event Grid com o Logic Apps para processar dados em qualquer local sem ter de escrever código. 

## <a name="how-much-does-event-grid-cost"></a>Quanto custa o Event Grid?

O Azure Event Grid tem um modelo de preços “pagar por evento”, de modo que só paga pelo que utilizar. As primeiras 100 000 operações por mês são gratuitas. As operações são definidas como eventos de entrada, correspondência avançada, tentativa de entrega e chamadas de gestão. Para obter detalhes, veja a [página de preços](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Passos seguintes

* [Route Storage Blob events](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) (Encaminhar eventos do Blob de Armazenamento)  
  Utilize o Event Grid para responder a eventos de blobs de armazenamento.
* [Criar e subscrever eventos personalizados](custom-event-quickstart.md)  
  Utilize o início rápido do Azure Event Grid para começar a enviar os seus próprios eventos personalizados para qualquer ponto final imediatamente.
* [Using Logic Apps as an Event Handler](monitor-virtual-machine-changes-event-grid-logic-app.md) (Utilizar o Logic Apps como Processador de Eventos)  
  Um tutorial sobre como criar uma aplicação com o Logic Apps para reagir a eventos enviados pelo Event Grid.
* [Transmitir macrodados em fluxo para um armazém de dados](event-grid-event-hubs-integration.md)  
  Um tutorial que utiliza as Funções do Azure para transmitir dados em fluxo dos Hubs de Eventos para o SQL Data Warehouse.
* [Event Grid REST API reference](/rest/api/eventgrid) (Referência da API REST do Event Grid)  
  Disponibiliza mais informações técnicas sobre o Azure Event Grid e uma referência para gerir subscrições de eventos, encaminhamento e filtragem.