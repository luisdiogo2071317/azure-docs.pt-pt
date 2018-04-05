---
title: Comparar o IoT Hub do Azure para Event Hubs do Azure | Microsoft Docs
description: Uma comparação dos serviços de IoT Hub e do Azure Hubs de eventos, realçando diferenças funcionais e casos de utilização. A comparação inclui protocolos suportados, gestão de dispositivos, monitorização, e carrega o ficheiro.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2018
ms.author: kgremban
ms.openlocfilehash: 303a2bde0a1e0b25ca6eb145e7b0cd6c91fff351
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Comparação do IoT Hub do Azure e Event Hubs do Azure

IoT Hub do Azure e Event Hubs do Azure são serviços em nuvem que podem ingerir grandes quantidades de dados e processar ou armazenar esses dados informações empresariais. Os dois serviços são semelhantes em que ambos os suportam processar os dados de telemetria e eventos com baixa latência e alta fiabilidade. No entanto, apenas o IoT Hub foi desenvolvido com as capacidades específicas necessárias para suportar internet à escala dos cenários de coisas. 

IoT Hub do Azure é o gateway de nuvem que liga dispositivos e recolhe dados de informações empresariais e automatização. Facilita transmitir dados para a nuvem e gerir os seus dispositivos à escala. Um differentiator importante entre IoT Hub e outros serviços de ingestão de dados é que o IoT Hub inclui funcionalidades que enriquecer a relação entre os seus dispositivos e os sistemas de back-end. Capacidades de comunicação bidirecional significam que ao receber dados dos dispositivos também pode enviar mensagens de volta para dispositivos para atualizar as propriedades ou invocar uma ação. Identidade ao nível do dispositivo ajuda a proteger o seu sistema. Distribuída informático move de lógica de serviço de nuvem para dispositivos de limite.

[Os Hubs de eventos do Azure] [ Azure Event Hubs] é um serviço de ingestão de eventos que pode processar e armazenar grandes quantidades de dados e a telemetria. Hubs de eventos foi concebido para ingestão de evento em grande escala, ambos no contexto do Centro de dados inter- e intra-datacenter cenários, mas não fornece capacidades avançadas do IoT específicos que estão disponíveis com o IoT Hub. Por esse motivo, não recomendamos que os Event Hubs para as suas soluções de IoT. 

A tabela seguinte fornece detalhes sobre como as duas camadas do IoT Hub comparam com os Event Hubs quando estiver a avaliá-los para capacidades de IoT. Para obter mais informações sobre os escalões básicos e padrão de IoT Hub, consulte [como escolher o escalão de IoT Hub direita][lnk-scaling].

| Capacidade de IoT | Escalão padrão de IoT Hub | Escalão básico de IoT Hub | Event Hubs |
| --- | --- | --- | --- |
| Mensagens do dispositivo-nuvem | ![Marcar][1] | ![Marcar][1] | ![Marcar][1] |
| Protocolos: AMQP de HTTPS, AMQP, através de websockets | ![Marcar][1] | ![Marcar][1] | ![Marcar][1] |
| Protocolos: MQTT, MQTT através de websockets | ![Marcar][1] | ![Marcar][1] |  |
| Identidade por dispositivo | ![Marcar][1] | ![Marcar][1] |  |
| Carregamento de ficheiros a partir de dispositivos | ![Marcar][1] | ![Marcar][1] |  |
| Serviço de Aprovisionamento do Dispositivo | ![Marcar][1] | ![Marcar][1] |  |
| Mensagens da nuvem para dispositivo | ![Marcar][1] |  |  |
| Dispositivo duplo e gestão de dispositivos | ![Marcar][1] |  |  |
| IoT Edge | ![Marcar][1] |  |  |

Mesmo se o caso de utilização única ingestão de dados do dispositivo para a nuvem, é vivamente recomendado utilizar o IoT Hub como fornece um serviço que foi concebido para conectividade de dispositivos de IoT. 

### <a name="next-steps"></a>Passos Seguintes

Para explorar ainda mais as capacidades do IoT Hub, consulte o [guia para programadores do IoT Hub][lnk-devguide]


[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md

<!--Image references-->
[1]: ./media/iot-hub-compare-event-hubs/ic195031.png