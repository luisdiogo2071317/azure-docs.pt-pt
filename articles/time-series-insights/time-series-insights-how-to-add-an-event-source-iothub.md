---
title: Como adicionar uma origem de evento do IoT Hub para o Azure Time Series Insights | Documentos da Microsoft
description: Este artigo descreve como adicionar uma origem de evento que está ligada a um IoT Hub para o seu ambiente do Time Series Insights
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: 3168fb6cf6e9b08fe4d2de921179129db241d153
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847172"
---
# <a name="how-to-add-an-iot-hub-event-source-to-time-series-insights-environment"></a>Como adicionar uma origem de evento do IoT Hub ao ambiente do Time Series Insights

Este artigo descreve como utilizar o portal do Azure para adicionar uma origem de evento que lê dados a partir de um IoT Hub em seu ambiente do Azure Time Series Insights (TSI).

> [!NOTE]
> Estas instruções aplicam-se para disponibilidade geral do TSI e ambientes de pré-visualização do TSI.

## <a name="prerequisites"></a>Pré-requisitos

* Crie um ambiente do TSI. Para obter mais informações, consulte [criar um ambiente do Azure TSI](time-series-insights-update-create-environment.md).
* Criar um Hub IoT. Para obter mais informações sobre os Hubs IoT, veja [criar um IoT Hub com o portal do Azure](../iot-hub/iot-hub-create-through-portal.md).
* O IoT Hub tem de ter os eventos de mensagem ativa a ser enviados na.
* Crie um grupo de consumidores dedicado no IoT Hub para o ambiente do TSI para consumir de. Cada origem de evento do TSI precisa ter seu próprio grupo de consumidores dedicado que não é partilhado com quaisquer outros consumidores. Se vários leitores consumirem eventos a partir do mesmo grupo de consumidores, todos os leitores provável ver falhas. Para obter detalhes, consulte a [Guia do programador do IoT Hub](../iot-hub/iot-hub-devguide.md).

### <a name="add-a-consumer-group-to-your-iot-hub"></a>Adicionar um grupo de consumidores ao seu IoT Hub

Grupos de consumidores são utilizados por aplicações para extrair dados de Hubs IoT do Azure. Forneça um grupo de consumidores dedicado para utilização por este ambiente de TSI apenas, fiável, ler dados do seu IoT Hub.

Para adicionar um novo grupo de consumidores ao seu IoT Hub, siga estes passos:

1. No portal do Azure, localize e abra o IoT Hub.
1. Sob o **configurações** cabeçalho, selecione **pontos finais incorporados**.

   ![IoT Hub, um][1]

1. Selecione o **eventos** ponto final e o **propriedades** é aberta a página.

1. Sob o **grupos de consumidores** cabeçalho, fornecer um novo nome exclusivo para o grupo de consumidores. Utilize este mesmo nome no ambiente do TSI ao criar uma nova origem de evento.

1. Selecione **guardar** para guardar o novo grupo de consumidor.

## <a name="add-a-new-event-source"></a>Adicionar uma nova origem de evento

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. Localize o seu ambiente existente do TSI. Clique em **todos os recursos** no menu no lado esquerdo do portal do Azure. Selecione o seu ambiente do TSI.

1. Sob o **topologia do ambiente** , clique em **origens de eventos**.

   ![Hub IoT dois][2]

1. Clique em **+ Adicionar**.

1. Fornecer uma **nome da origem de evento** exclusivo para este ambiente de TSI, tal como **fluxo de eventos**.

   ![Hub IoT três][3]

1. Selecione o **origem** como **IoT Hub**.

1. Selecione o adequado **importar opção**.

   * Escolher **utilize o IoT Hub a partir de subscrições disponíveis** se já tiver um IoT Hub das suas subscrições. Esta é a abordagem mais fácil.
   * Escolher **definições de fornecer o IoT Hub manualmente** quando o IoT Hub é externo para as suas subscrições ou pretender escolher opções avançadas.

1. Se tiver selecionado o **utilize o IoT Hub a partir de subscrições disponíveis** opção, a tabela seguinte explica cada propriedade necessária:

   ![Hub IoT quatro][4]

   | Propriedade | Descrição |
   | --- | --- |
   | ID da subscrição | Selecione a subscrição na qual este IoT Hub foi criado.
   | Nome do IoT Hub | Selecione o nome do IoT Hub.
   | Nome de política do IoT Hub | Selecione a política de acesso partilhado, o que pode ser encontrada no separador de definições do IoT Hub. Cada política de acesso partilhado tem um nome, as permissões que definir e chaves de acesso. A política de acesso partilhado para a origem do evento *tem* ter **serviço ligar** permissões.
   | Chave de política do IoT Hub | A chave é pré-preenchida.
   | Grupo de consumidores do IoT Hub | O grupo de consumidor ler eventos a partir do IoT Hub. É altamente recomendado para utilizar um grupo de consumidores dedicado para a origem do evento.
   | Formato de serialização de eventos | JSON é a serialização só estão disponível neste momento. As mensagens de eventos tem de estar no seguinte formato, ou podem ser lidos sem dados. |
   | Nome da propriedade Timestamp | Para determinar este valor, precisa entender o formato de mensagem dos dados da mensagem enviados para o IoT Hub. Este valor é o **nome** da propriedade de evento específico dos dados de mensagem que pretende utilizar como o carimbo de hora do evento. O valor diferencia maiúsculas de minúsculas. Quando deixado em branco, o **tempo de colocar em fila de eventos** dentro do evento, origem é utilizada como o carimbo de hora do evento. |

1. Se tiver selecionado o **definições de fornecer o IoT Hub manualmente** opção, a tabela seguinte explica cada propriedade necessária:

   | Propriedade | Descrição |
   | --- | --- |
   | ID da subscrição | A subscrição na qual este IoT Hub foi criado.
   | Grupo de recursos | O nome do grupo de recursos no qual este IoT Hub foi criado.
   | Nome do IoT Hub | O nome do IoT Hub. Quando criou o seu IoT Hub, também lhe atribuiu um nome específico.
   | Nome de política do IoT Hub | A política de acesso partilhado, que pode ser criada no separador de definições do IoT Hub. Cada política de acesso partilhado tem um nome, as permissões que definir e chaves de acesso. A política de acesso partilhado para a origem do evento *tem* ter **serviço ligar** permissões.
   | Chave de política do IoT Hub | A chave de acesso partilhado utilizada para autenticar o acesso ao espaço de nomes do Service Bus. Escreva a chave primária ou secundária aqui.
   | Grupo de consumidores do IoT Hub | O grupo de consumidor ler eventos a partir do IoT Hub. É altamente recomendado para utilizar um grupo de consumidores dedicado para a origem do evento.
   | Formato de serialização de eventos | JSON é a serialização só estão disponível neste momento. As mensagens de eventos tem de estar no seguinte formato, ou podem ser lidos sem dados. |
   | Nome da propriedade Timestamp | Para determinar este valor, precisa entender o formato de mensagem dos dados da mensagem enviados para o IoT Hub. Este valor é o **nome** da propriedade de evento específico dos dados de mensagem que pretende utilizar como o carimbo de hora do evento. O valor diferencia maiúsculas de minúsculas. Quando deixado em branco, o **tempo de colocar em fila de eventos** dentro do evento, origem é utilizada como o carimbo de hora do evento. |

1. Adicione o nome de grupo consumidor dedicado TSI adicionado ao seu IoT Hub.

1. Selecione **criar** para adicionar a nova origem de evento.

   ![Hub IoT cinco][5]

1. Depois de criar a origem de eventos, o Time Series Insights vai começar a transmitir dados para o seu ambiente automaticamente.

## <a name="next-steps"></a>Passos Seguintes

* [Definir políticas de acesso de dados](time-series-insights-data-access.md) para proteger os dados.
* [Enviar eventos](time-series-insights-send-events.md) para a origem do evento.
* Aceder ao seu ambiente no [Explorador do Time Series Insights](https://insights.timeseries.azure.com).

<!-- Images -->
[1]: media/time-series-insights-how-to-add-an-event-source-iothub/iothub_one.png
[2]: media/time-series-insights-how-to-add-an-event-source-iothub/iothub_two.png
[3]: media/time-series-insights-how-to-add-an-event-source-iothub/iothub_three.png
[4]: media/time-series-insights-how-to-add-an-event-source-iothub/iothub_four.png
[5]: media/time-series-insights-how-to-add-an-event-source-iothub/iothub_five.png