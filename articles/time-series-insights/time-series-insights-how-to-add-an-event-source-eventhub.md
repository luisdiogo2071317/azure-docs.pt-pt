---
title: Como adicionar uma origem de evento do Hub de eventos para o Azure Time Series Insights | Documentos da Microsoft
description: Este artigo descreve como adicionar uma origem de evento que está ligada a um Hub de eventos para o seu ambiente do Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: 93076a8b0d11e05b86c20868d4bc23f4826f107f
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52832450"
---
# <a name="how-to-add-an-event-hub-event-source-to-time-series-insights-environment"></a>Como adicionar uma origem de evento do Hub de eventos ao ambiente do Time Series Insights

Este artigo descreve como utilizar o portal do Azure para adicionar uma origem de evento que lê dados a partir de um Hub de eventos em seu ambiente do Time Series Insights.

> [!NOTE]
> Estas instruções aplicam-se em ambos os DG de informações de série de tempo e ambientes de pré-visualização privada de atualização do Time Series Insights.

## <a name="prerequisites"></a>Pré-requisitos

- Crie um ambiente do Time Series Insights. Para obter mais informações, consulte [criar um ambiente do Azure Time Series Insights](./time-series-insights-update-create-environment.md).
- Criar um Hub de Eventos. Para obter mais informações sobre os Hubs de eventos, consulte [criar um espaço de nomes de Hubs de eventos e um hub de eventos com o portal do Azure](../event-hubs/event-hubs-create.md).
- O Hub de eventos tem de ter os eventos de mensagem ativa a ser enviados na. Para obter mais informações, consulte [enviar eventos para Hubs de eventos usando o .NET Framework](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Crie um grupo de consumidores dedicado no Hub de eventos para o ambiente do Time Series Insight para consumir de. Cada origem de eventos do Time Series Insights tem de ter seu próprio grupo de consumidores dedicado que não é partilhado com quaisquer outros consumidores. Se vários leitores consumirem eventos a partir do mesmo grupo de consumidores, todos os leitores provável ver falhas. Tenha em atenção que também existe um limite de 20 grupos de consumidores do Hub de eventos. Para obter detalhes, consulte a [guia de programação dos Hubs de eventos](../event-hubs/event-hubs-programming-guide.md).

### <a name="add-a-consumer-group-to-your-event-hub"></a>Adicionar um grupo de consumidores ao seu Hub de eventos

Grupos de consumidores são utilizados por aplicações para extrair dados de Hubs de eventos do Azure. Forneça um grupo de consumidores dedicado para utilização por este ambiente de Time Series Insights apenas, fiável ler dados a partir do seu Hub de eventos.

Para adicionar um novo grupo de consumidores do seu Hub de eventos, siga estes passos:

1. No portal do Azure, localize e abra o Hub de eventos.

1. Sob o **entidades** cabeçalho, selecione **grupos de consumidores**.

   ![Hub de eventos - adicionar um grupo de consumidores](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png)

1. Selecione **+ grupo de consumidores** para adicionar um novo grupo de consumidor. 

1. Sobre o **grupos de consumidores** , indique um novo exclusivo **nome**.  Utilize este nome de mesmo ao criar uma nova origem de evento no ambiente do Time Series Insights.

1. Selecione **criar** para criar o novo grupo de consumidores.

## <a name="add-a-new-event-source"></a>Adicionar uma nova origem de evento

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. Localize o seu ambiente do Time Series Insights existente. Clique em **todos os recursos** no menu no lado esquerdo do portal do Azure. Selecione o seu ambiente do Time Series Insights.

1. Sob o **topologia do ambiente** , clique em **origens de eventos**.

   ![Origens de evento + adicionar](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png)

1. Clique em **+ Adicionar**.

1. Fornecer uma **nome da origem de evento** exclusivo para este ambiente do Time Series Insights, tal como **fluxo de eventos**.

   ![Preencha os primeiros três parâmetros na forma.](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png)

1. Selecione o **origem** como **Hub de eventos**.

1. Selecione o adequado **importar opção**.
   - Escolher **Hub de eventos de utilização de subscrições disponíveis** se já tiver um Hub de eventos existente em uma das suas subscrições. Esta é a abordagem mais fácil.
   - Escolher **definições do Hub de eventos de fornecer manualmente** quando o Hub de eventos é externo para as suas subscrições ou pretender escolher opções avançadas.

1. Se tiver selecionado o **Hub de eventos de utilização de subscrições disponíveis** opção, a tabela seguinte explica cada propriedade necessária:

   ![Detalhes de hub de eventos e de subscrição](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)

   | Propriedade | Descrição |
   | --- | --- |
   | ID da subscrição | Selecione a subscrição na qual este hub de eventos foi criado.
   | Espaço de nomes do Service Bus | Selecione o espaço de nomes do Service Bus que contém o Hub de eventos.
   | Nome do hub de eventos | Selecione o nome do Hub de eventos.
   | Nome de política do hub de eventos | Selecione a política de acesso partilhado, que pode ser criada no separador Configurar do Hub de eventos. Cada política de acesso partilhado tem um nome, as permissões que definir e chaves de acesso. A política de acesso partilhado para a origem do evento *tem* ter **ler** permissões.
   | Chave de política do hub de eventos | O valor da chave pode ser pré-preenchidas.
   | Grupo de consumidores do hub de eventos | O grupo de consumidor ler eventos do Hub de eventos. É altamente recomendado para utilizar um grupo de consumidores dedicado para a origem do evento. |
   | Formato de serialização de eventos | JSON é a serialização só estão disponível neste momento. As mensagens de eventos tem de estar no seguinte formato, ou podem ser lidos sem dados. |
   | Nome da propriedade Timestamp | Para determinar este valor, precisa entender o formato de mensagem dos dados da mensagem enviados para o Hub de eventos. Este valor é o **nome** da propriedade de evento específico dos dados de mensagem que pretende utilizar como o carimbo de hora do evento. O valor diferencia maiúsculas de minúsculas. Quando deixado em branco, o **tempo de colocar em fila de eventos** dentro do evento, origem é utilizada como o carimbo de hora do evento. |

1. Se tiver selecionado o **definições do Hub de eventos de fornecer manualmente** opção, a tabela seguinte explica cada propriedade necessária:

   | Propriedade | Descrição |
   | --- | --- |
   | ID da subscrição | A subscrição na qual este hub de eventos foi criado.
   | Grupo de recursos | O grupo de recursos no qual este hub de eventos foi criado.
   | Espaço de nomes do Service Bus | Um espaço de nomes do Service Bus é um contentor para um conjunto de entidades de mensagens. Quando tiver criado um novo Hub de eventos, também criou um espaço de nomes do Service Bus.
   | Nome do hub de eventos | O nome do seu Hub de eventos. Quando criou o seu hub de eventos, também lhe atribuiu um nome específico.
   | Nome de política do hub de eventos | A política de acesso partilhado, que pode ser criada no separador Configurar do Hub de eventos. Cada política de acesso partilhado tem um nome, as permissões que definir e chaves de acesso. A política de acesso partilhado para a origem do evento *tem* ter **ler** permissões.
   | Chave de política do hub de eventos | A chave de acesso partilhado utilizada para autenticar o acesso ao espaço de nomes do Service Bus. Escreva a chave primária ou secundária aqui.
   | Grupo de consumidores do hub de eventos | O grupo de consumidor ler eventos do Hub de eventos. É altamente recomendado para utilizar um grupo de consumidores dedicado para a origem do evento.
   | Formato de serialização de eventos | JSON é a serialização só estão disponível neste momento. As mensagens de eventos tem de estar no seguinte formato, ou podem ser lidos sem dados. |
   | Nome da propriedade Timestamp | Para determinar este valor, precisa entender o formato de mensagem dos dados da mensagem enviados para o Hub de eventos. Este valor é o **nome** da propriedade de evento específico dos dados de mensagem que pretende utilizar como o carimbo de hora do evento. O valor diferencia maiúsculas de minúsculas. Quando deixado em branco, o **tempo de colocar em fila de eventos** dentro do evento, origem é utilizada como o carimbo de hora do evento. |

1. Adicione o nome de grupo consumidor dedicado TSI adicionado ao seu hub de eventos.

1. Selecione **criar** para adicionar a nova origem de evento.

   ![Clique em Criar](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png)

   Depois de criar a origem de eventos, o Time Series Insights vai começar a transmitir dados para o seu ambiente automaticamente.

## <a name="next-steps"></a>Passos Seguintes

[Definir políticas de acesso de dados](time-series-insights-data-access.md) para proteger os dados.

[Enviar eventos](time-series-insights-send-events.md) para a origem do evento.

Aceder ao seu ambiente no [Explorador do Time Series Insights](https://insights.timeseries.azure.com).
