---
title: Planejar seu ambiente do Azure Time Series Insights (pré-visualização) | Documentos da Microsoft
description: Planejar seu ambiente do Azure Time Series Insights (pré-visualização)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: c385d10aac01c844f1d4b390c0bb3d064b9befa3
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52878708"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Planejar seu ambiente do Azure Time Series Insights (pré-visualização)

Este artigo descreve as melhores práticas para planear e começar a utilizar rapidamente o Azure Time Series Insights (pré-visualização).

## <a name="best-practices-for-planning-and-preparation"></a>Melhores práticas para o planejamento e preparação

Para começar a utilizar com o Time Series Insights (TSI), é melhor se compreender o seguinte:

* O que estiver recebendo Quando aprovisiona um ambiente de TSI (pré-visualização).
* O que sua **IDs de série de tempo** e **Timestamp** são propriedades.
* Nova **modelo de série de tempo** é e como criar seus próprios.
* Como enviar eventos com eficiência em JSON.  
* Opções de recuperação de desastre de negócio TSI.

A atualização do Time Series Insights emprega um modelo de negócio pay as you go.  Para obter mais informações sobre custos e a capacidade, consulte [preços de Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-time-series-insights-preview-environment"></a>O ambiente do Time Series Insights (pré-visualização)

Quando aprovisiona um ambiente de TSI (pré-visualização), cria dois recursos do Azure:

* Ambiente de TSI (pré-visualização)
* Conta do armazenamento do Azure para fins gerais V1

Para começar, terá três itens adicionais.  A primeira é uma [modelo de série de tempo](./time-series-insights-update-tsm.md), o segundo é um [origem de eventos ligado ao Time Series Insights](./time-series-insights-how-to-add-an-event-source-iothub.md), e o terceiro é [eventos a ser encaminhados para a origem do evento](./time-series-insights-send-events.md) que são ambos mapeado para o modelo e estão no formato JSON válido.  

## <a name="configure-your-time-series-ids-and-timestamp-properties"></a>Configurar as propriedades de IDs de série de tempo e Timestamp

Para criar um novo ambiente de TSI, selecione um **ID de série de tempo**. Ao fazê-lo então, atua como uma partição lógica para os seus dados. Como observado, certifique-se de ter sua **IDs de série de tempo** pronto.

> [!IMPORTANT]
> **IDs de série de tempo** estão **imutável** e **não é possível alterar mais tarde**. Verifique se cada antes da seleção final e utilizar pela primeira vez.

Pode selecionar até **três** (3) chaves para distinguir exclusivamente os seus recursos. Leitura a [melhores práticas para a escolha de um ID de série de tempo](./time-series-insights-update-how-to-id.md) e [armazenamento e de entrada](./time-series-insights-update-storage-ingress.md) artigos para obter mais informações.

O **Timestamp** propriedade também é muito importante. Pode designar esta propriedade quando adicionar origens de eventos. Cada origem de evento tem opcional **Timestamp** propriedade que é utilizado para origens de eventos de controle ao longo do tempo. **Timestamp** valores diferenciam maiúsculas de minúsculas e têm de ser formatados a especificação individuais de cada origem de evento.

> [!TIP]
> Verifique se os requisitos de formatação e análise para origens de evento.

Quando deixado em branco, o **tempo de colocar em fila de eventos** de um evento a origem é utilizada como o evento **Timestamp**. Se estiverem a enviar dados históricos ou eventos em lote, provavelmente encontrará personalizando os **Timestamp** propriedade para ser mais útil que não o predefinido **hora do evento colocar em fila**. Para obter mais informações, leia sobre [como adicionar origens de eventos no IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md).  

## <a name="understand-the-time-series-model"></a>Compreender a série de tempo modelo

Agora pode configurar o seu ambiente de TSI **modelo de série de tempo**. O novo modelo torna mais fácil localizar e analisar dados de IoT. Ele faz isso, permitindo que a organização, manutenção e enriquecimento de dados de séries de tempo e ajuda a preparar conjuntos de dados de preparado para o consumidor. O modelo utiliza **IDs de série de tempo**, que mapeiam para uma instância de associar o exclusivo recurso variáveis (conhecidas como tipos) e hierarquias. Saiba mais sobre a nova [modelo de série de tempo](./time-series-insights-update-tsm.md).

O modelo é dinâmico, para que ele pode ser criado em qualquer altura. No entanto, poderá começar a utilizar mais rapidamente se ele tiver criado e carregado antes de a começar a enviar dados para o TSI. Para criar o seu modelo, reveja os [como utilizar TSM](./time-series-insights-update-how-to-tsm.md) artigo.

Para muitos clientes, esperamos que o **modelo de série de tempo** para mapear para um modelo de recurso existente ou o sistema ERP já em vigor. Para os clientes que não têm um modelo existente, é uma experiência de usuários predefinidos [fornecida](https://github.com/Microsoft/tsiclient) para começar a trabalhar rapidamente. Pode imaginar como um modelo pode ajudá-lo ao visualizar nosso [ambiente de demonstração de exemplo](https://insights.timeseries.azure.com/preview/demo).  

## <a name="shaping-your-events"></a>Formatação de seus eventos

É importante verificar a forma como os eventos são enviados para o TSI. O ideal é que os seus eventos irão ser desnormalizados bem e eficiente.

Uma boa regra prática:

* Metadados devem ser armazenados na sua **modelo de série de tempo**
* **Modo de série de tempo;**  campos de instância e eventos devem informações apenas necessárias, tais como:
  * **ID de série de tempo**
  * **Timestamp**

Reveja os [como eventos de forma](./time-series-insights-send-events.md#json) artigo para obter mais detalhes.

## <a name="business-disaster-recovery"></a>Recuperação após desastre de negócio

Como um serviço do Azure, o TSI é um serviço de elevada disponibilidade (HA) com redundâncias ao nível da região do Azure. É necessária nenhuma configuração para utilizar estas funcionalidades inerentes. A plataforma Microsoft Azure também inclui recursos para ajudar a criar soluções com capacidades de recuperação (DR) após desastre ou entre regiões de disponibilidade. Se quiser fornecer global, entre regiões HA para dispositivos ou utilizadores, tirar partido destas funcionalidades de DR do Azure. O artigo [orientações técnicas de continuidade do negócio de Azure](https://docs.microsoft.com/azure/resiliency/resiliency-technical-guidance) descreve as funcionalidades incorporadas no Azure para a continuidade do negócio e de DR. O [desastre](https://docs.microsoft.com/azure/architecture/resiliency/index) recuperação e de elevada disponibilidade para o documento de aplicações do Azure fornece uma orientação de arquitetura sobre estratégias para aplicações do Azure alcançar a HA e DR.

> [!NOTE]
> O Azure Time Series Insights não tem BCDR incorporada.
> Por predefinição no armazenamento do Azure, IoT Hub do Azure e os Hubs de eventos tem recuperação incorporada.

Para obter mais informações:

* Leia sobre [redundância do armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-redundancy).
* Leia sobre [HA DR do Hub IoT](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr).
* Leia sobre [políticas do Hub de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).

No entanto, os clientes que necessitam de BCDR ainda podem implementar uma estratégia de recuperação através da criação de um ambiente de TSI segundo numa região do Azure de cópia de segurança. Os clientes enviam eventos para este ambiente secundário, da origem de eventos primário, tirando partido de um segundo grupo de consumidores dedicado e diretrizes BCDR essa origem de evento.

Seguem-se os passos específicos para fazer isso:

1. Crie um ambiente numa segunda região. Leia sobre [ambientes de TSI](./time-series-insights-get-started.md).
1. Criar um segundo grupo de consumidores dedicado para a origem do evento e ligar essa origem de evento para o novo ambiente. Certifique-se de que designar o grupo de consumidores dedicado, segundo. Saiba mais com o [documentação do IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) ou o [documentação do Hub de eventos](./time-series-insights-data-access.md).
1. Se a região primária está afetado durante um incidente de desastres, redirecionar operações para o ambiente do TSI cópia de segurança.

> [!IMPORTANT]
> * Tenha em atenção que possa encontrar um atraso na eventualidade de uma ativação pós-falha.
> * Ativação pós-falha também pode causar um pico de mometary no processamento de mensagens, como operações são reencaminhadas.
> * Para obter mais informações, consulte [atenuação de latência em TSI](./time-series-insights-environment-mitigate-latency.md).

## <a name="next-steps"></a>Passos Seguintes

Leitura a [armazenamento do Azure TSI (pré-visualização) e de entrada](./time-series-insights-update-storage-ingress.md).

Leia sobre [modelação de dados](./time-series-insights-update-tsm.md).