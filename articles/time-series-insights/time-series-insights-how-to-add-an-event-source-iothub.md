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
ms.date: 11/21/2017
ms.openlocfilehash: b6beecbf64cee925f62ac4c82919926fcb79940a
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627377"
---
# <a name="how-to-add-an-iot-hub-event-source-to-time-series-insights-environment"></a>Como adicionar uma origem de evento do IoT Hub ao ambiente do Time Series Insights
Este artigo descreve como utilizar o portal do Azure para adicionar uma origem de evento que lê dados a partir de um IoT Hub em seu ambiente do Time Series Insights.

## <a name="prerequisites"></a>Pré-requisitos
- Crie um ambiente do Time Series Insights. Para obter mais informações, consulte [criar um ambiente do Azure Time Series Insights](time-series-insights-get-started.md) 
- Criar um Hub IoT. Para obter mais informações sobre os Hubs IoT, consulte [criar um IoT Hub com o portal do Azure](../iot-hub/iot-hub-create-through-portal.md)
- O IoT Hub tem de ter os eventos de mensagem ativa a ser enviados na.
- Crie um grupo de consumidores dedicado no IoT Hub para o ambiente do Time Series Insight para consumir de. Cada origem de eventos do Time Series Insights tem de ter seu próprio grupo de consumidores dedicado que não é partilhado com quaisquer outros consumidores. Se vários leitores consumirem eventos a partir do mesmo grupo de consumidores, todos os leitores provável ver falhas. Para obter detalhes, consulte a [Guia do programador do IoT Hub](../iot-hub/iot-hub-devguide.md).

### <a name="add-a-consumer-group-to-your-iot-hub"></a>Adicionar um grupo de consumidores ao seu IoT Hub
Grupos de consumidores são utilizados por aplicações para extrair dados de Hubs IoT do Azure. Forneça um grupo de consumidores dedicado para utilização por este ambiente de Time Series Insights apenas, fiável, ler dados do seu IoT Hub.

Para adicionar um novo grupo de consumidores ao seu IoT Hub, siga estes passos:
1. No portal do Azure, localize e abra o IoT Hub.

2. Sob o **Messaging** cabeçalho, selecione **pontos de extremidade**. 

   ![Adicionar um grupo de consumidores](media/time-series-insights-how-to-add-an-event-source-iothub/5-add-consumer-group.png)

3. Selecione o **eventos** ponto final e o **propriedades** é aberta a página.

4. Sob o **grupos de consumidores** cabeçalho, fornecer um novo nome exclusivo para o grupo de consumidores. Utilize este mesmo nome no ambiente do Time Series Insights durante a criação de uma nova origem de evento.

5. Selecione **guardar** para guardar o novo grupo de consumidor.

## <a name="add-a-new-event-source"></a>Adicionar uma nova origem de evento
1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Localize o seu ambiente do Time Series Insights existente. Clique em **todos os recursos** no menu no lado esquerdo do portal do Azure. Selecione o seu ambiente do Time Series Insights.

3. Sob o **topologia do ambiente** , clique em **origens de eventos**.
   ![Origens de evento + adicionar](media/time-series-insights-how-to-add-an-event-source-iothub/1-event-sources.png)

4. Clique em **+ Adicionar**.

5. Fornecer uma **nome da origem de evento** exclusivo para este ambiente do Time Series Insights, tal como **fluxo de eventos**.

   ![Preencha os primeiros três parâmetros na forma.](media/time-series-insights-how-to-add-an-event-source-iothub/2-import-option.png)

6. Selecione o **origem** como **IoT Hub**.

7. Selecione o adequado **importar opção**. 
   - Escolher **utilize o IoT Hub a partir de subscrições disponíveis** se já tiver um IoT Hub das suas subscrições. Esta é a abordagem mais fácil.
   - Escolher **definições de fornecer o IoT Hub manualmente** quando o IoT Hub é externo para as suas subscrições ou pretender escolher opções avançadas. 

8. Se tiver selecionado o **utilize o IoT Hub a partir de subscrições disponíveis** opção, a tabela seguinte explica cada propriedade necessária:

   ![Detalhes de hub de eventos e de subscrição](media/time-series-insights-how-to-add-an-event-source-iothub/3-new-event-source.png)

   | Propriedade | Descrição |
   | --- | --- |
   | ID da subscrição | Selecione a subscrição na qual este IoT Hub foi criado.
   | Nome do IoT Hub | Selecione o nome do IoT Hub.
   | Nome de política do IoT Hub | Selecione a política de acesso partilhado, o que pode ser encontrada no separador de definições do IoT Hub. Cada política de acesso partilhado tem um nome, as permissões que definir e chaves de acesso. A política de acesso partilhado para a origem do evento *tem* ter **serviço ligar** permissões.
   | Chave de política do IoT Hub | A chave é pré-preenchida.
   | Grupo de consumidores do IoT Hub | O grupo de consumidor ler eventos a partir do IoT Hub. É altamente recomendado para utilizar um grupo de consumidores dedicado para a origem do evento.
   | Formato de serialização de eventos | JSON é a serialização só estão disponível neste momento. As mensagens de eventos tem de estar no seguinte formato, ou podem ser lidos sem dados. |
   | Nome da propriedade Timestamp | Para determinar este valor, precisa entender o formato de mensagem dos dados da mensagem enviados para o IoT Hub. Este valor é o **nome** da propriedade de evento específico dos dados de mensagem que pretende utilizar como o carimbo de hora do evento. O valor diferencia maiúsculas de minúsculas. Quando deixado em branco, o **tempo de colocar em fila de eventos** dentro do evento, origem é utilizada como o carimbo de hora do evento. |

9. Se tiver selecionado o **definições de fornecer o IoT Hub manualmente** opção, a tabela seguinte explica cada propriedade necessária:

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

10. Adicione o nome de grupo consumidor dedicado TSI adicionado ao seu IoT Hub.

11. Selecione **criar** para adicionar a nova origem de evento.

   ![Clique em Criar](media/time-series-insights-how-to-add-an-event-source-iothub/4-create-button.png)

   Depois de criar a origem de eventos, o Time Series Insights vai começar a transmitir dados para o seu ambiente automaticamente.

## <a name="next-steps"></a>Passos Seguintes
- [Definir políticas de acesso de dados](time-series-insights-data-access.md) para proteger os dados.
- [Enviar eventos](time-series-insights-send-events.md) para a origem do evento.
- Aceder ao seu ambiente no [Explorador do Time Series Insights](https://insights.timeseries.azure.com).
