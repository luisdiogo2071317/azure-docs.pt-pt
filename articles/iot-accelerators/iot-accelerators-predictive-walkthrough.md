---
title: Descrição geral do acelerador de soluções de Manutenção Preditiva - Azure | Microsoft Docs
description: Uma descrição geral do acelerador de solução da manutenção preditiva do IoT do Azure.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: dobett
ms.openlocfilehash: 6bd61f301fa4468ef1d308d4da275c760ee09dc0
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37088638"
---
# <a name="predictive-maintenance-solution-accelerator-overview"></a>Descrição geral do acelerador de soluções de Manutenção Preditiva

O acelerador de soluções de Manutenção Preditiva é uma solução ponto a ponto para um cenário de negócio que prevê o momento em que poderá ocorrer uma falha. Pode utilizar, de forma pró-ativa, este acelerador de soluções para atividades como a otimização da manutenção. A solução combina os serviços de aceleradores de soluções principais do Azure IoT, como o Hub IoT, o Stream Analytics e uma área de trabalho do [Azure Machine Learning][lnk-machine-learning]. Esta área de trabalho contém um modelo, com base num conjunto de dados de exemplo público, para prever a Vida Útil Remanescente (RUL) de um motor de aeronave. A solução implementa completamente o cenário empresarial IoT como um ponto de partida para que possa planear e implementar uma solução que satisfaça os seus próprios requisitos de negócio.

## <a name="logical-architecture"></a>Arquitetura lógica

O diagrama que se segue descreve os componentes lógicos do acelerador de soluções:

![][img-architecture]

Os itens a azul são os serviços do Azure aprovisionados na região em que implementou o acelerador de soluções. Apresenta a lista de regiões onde pode implementar o acelerador de soluções na [página aprovisionamento][lnk-azureiotsuite].

O item a verde é um dispositivo simulado que representa o motor de uma aeronave. Pode saber mais sobre estes dispositivos simulados na secção [Dispositivos simulados](#simulated-devices) seguinte.

Os itens a cinzento representam componentes que implementam capacidades de *gestão do dispositivo*. A atual versão do acelerador de soluções de Manutenção Preditiva não aprovisiona estes recursos. Para saber mais sobre a gestão de dispositivos, consulte o [acelerador de solução de monitorização remota][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Dispositivos simulados

No acelerador de soluções, um dispositivo simulado representa o motor de uma aeronave. A solução é aprovisionada com dois motores que mapeiam para uma única aeronave. Cada motor emite quatro tipos de telemetria: Sensor 9, Sensor 11, Sensor 14 e Sensor 15, que fornecem os dados necessários para que o modelo do Machine Learning possa calcular a RUL desse motor. Cada dispositivo simulado envia as seguintes mensagens de telemetria ao IoT Hub:

*Ciclo de contagem*. Um ciclo representa um voo concluído com uma duração entre duas a dez horas. Durante o voo, os dados telemétricos são capturados a cada meia hora.

*Telemetria*. Existem quatro sensores que representam os atributos do motor. Os sensores são geralmente denominados Sensor 9, Sensor 11, Sensor 14 e Sensor 15. Estes quatro sensores representam telemetria suficiente para obter resultados úteis a partir do modelo de RUL. O modelo utilizado no acelerador de soluções é criado a partir de um conjunto de dados públicos, que inclui dados do sensor do motor. Para obter mais informações sobre a criação do modelo a partir do conjunto de dados original, consulte o artigo [Cortana Intelligence Gallery Predictive Maintenance Template (Modelo de Manutenção Preditiva do Cortana Intelligence Gallery)][lnk-cortana-analytics].

Os dispositivos simulados podem processar os seguintes comandos enviados a partir do hub IoT na solução:

| Comando | Descrição |
| --- | --- |
| StartTelemetry |Controla o estado da simulação.<br/>Inicia o dispositivo que envia a telemetria |
| StopTelemetry |Controla o estado da simulação.<br/>Para o dispositivo que envia a telemetria |

O IoT Hub reconhece o comando do dispositivo.

## <a name="azure-stream-analytics-job"></a>Tarefa do Azure Stream Analytics

**Tarefa: Telemetria** intervém no fluxo de telemetria de entrada do dispositivo através de duas instruções:

* A primeira seleciona toda a telemetria dos dispositivos e envia esses dados ao armazenamento de blobs. Aí, são visualizados na aplicação Web.
* A segunda calcula os valores médios do sensor numa janela deslizante e envia esses dados através do Hub de Eventos a um **processador de eventos**.

## <a name="event-processor"></a>Processador de eventos
O **anfitrião do processador de eventos** executa uma Tarefa Web do Azure. O **processador de eventos** obtém os valores médios do sensor para um ciclo concluído. Em seguida, transmite esses valores para uma API que expõe o modelo formado para calcular a RUL de um motor. A API é exposta por uma área de trabalho do Machine Learning que está aprovisionada como parte da solução.

## <a name="machine-learning"></a>Machine Learning
O componente de Machine Learning utiliza um modelo derivado dos dados recolhidos de motores de aeronave reais. Pode navegar para a área de trabalho do Machine Learning a partir do mosaico da sua solução na página [azureiotsuite.com][lnk-azureiotsuite]. O mosaico está disponível quando a solução está no estado **Pronto**.


## <a name="next-steps"></a>Passos Seguintes
Agora que viu os principais componentes do acelerador de soluções de Manutenção Preditiva, deverá personalizá-los.

Também pode explorar algumas das outras funcionalidades e capacidades dos aceleradores de soluções do IoT:

* [Perguntas mais frequentes sobre aceleradores de soluções do IoT][lnk-faq]
* [Segurança de IoT desde o início][lnk-security-groundup]

[img-architecture]: media/iot-accelerators-predictive-walkthrough/architecture.png

[lnk-remote-monitoring]: iot-accelerators-remote-monitoring-explore.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsolutions.com/
[lnk-faq]: iot-accelerators-faq.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/