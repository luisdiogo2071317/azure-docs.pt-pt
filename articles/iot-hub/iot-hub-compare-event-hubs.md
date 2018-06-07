---
title: Comparar o IoT Hub do Azure para Event Hubs do Azure | Microsoft Docs
description: Uma comparação dos serviços de IoT Hub e do Azure Hubs de eventos, realçando diferenças funcionais e casos de utilização. A comparação inclui protocolos suportados, gestão de dispositivos, monitorização, e carrega o ficheiro.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: kgremban
ms.openlocfilehash: 9ad95071de07777e38533ecec9e8558841d8b1ca
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633966"
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>Ligação dos dispositivos de IoT no Azure: o IoT Hub e do Event Hubs

O Azure oferece serviços desenvolvidos especificamente para diversos tipos de comunicação para o ajudar a ligar os dados para a capacidade da nuvem e de conectividade. IoT Hub do Azure e Event Hubs do Azure são serviços em nuvem que podem ingerir grandes quantidades de dados e processar ou armazenar esses dados informações empresariais. Os dois serviços são semelhantes em que ambos os suportam ingestão de dados com baixa latência e alta fiabilidade, mas que foram concebidos para diferentes fins. IoT Hub foi desenvolvido especificamente para abordar os requisitos exclusivos da ligação de dispositivos de IoT, à escala, para a nuvem do Azure, enquanto os Hubs de eventos foi concebido para transmissão em fluxo de macrodados. Esta é a razão pela qual a Microsoft recomenda a utilização do Azure IoT Hub para ligar dispositivos de IoT no Azure

IoT Hub do Azure é o gateway de nuvem que liga dispositivos de IoT para recolher dados de informações empresariais de unidade e automatização. Além disso, o IoT Hub inclui funcionalidades que enriquecer a relação entre os seus dispositivos e os sistemas de back-end. Comunicação bidirecional capacidades significam que ao receber dados dos dispositivos, pode também enviar comandos e as políticas para dispositivos, por exemplo, para atualizar as propriedades ou da invocação de ações de gestão de dispositivos.  Conectividade esta nuvem para o dispositivo também for ligado a capacidade de entrega de nuvem intelligence para os seus dispositivos de limite com o Azure IoT Edge importante. A identidade de nível de dispositivo exclusiva fornecido pelo IoT Hub ajuda melhorar a segurança solução IoT contra potenciais ataques. 

[Os Hubs de eventos do Azure] [ Azure Event Hubs] são os macrodados transmissão em fluxo de serviço do Azure. Foi concebido para cenários de transmissão em fluxo de dados de alto débito para onde os clientes podem enviar billions de pedidos por dia. Utiliza um modelo de consumidor particionado para ampliar a sua transmissão em fluxo e está integrado de serviços de análise do Azure, incluindo Databricks, Stream Analytics, ADLS e HDInsight de macrodados e Hubs de eventos. Com funcionalidades como a captura de Hubs de eventos e Inflate de automática, este serviço foi concebido para suportar as suas aplicações de macrodados e soluções. Além disso, o IoT Hub tira partido dos Event Hubs para o caminho do fluxo de telemetria, para que a sua solução IoT também beneficia de potência imenso dos Event Hubs.

Para resumir, embora ambas as soluções são concebidas para ingestão de dados numa grande escala, apenas o IoT Hub disponibiliza IoT específicos capacidades avançadas que foram concebidas para si maximizar o valor de negócio de ligar os seus dispositivos de IoT a nuvem do Azure.  Se apenas é a partir da sua viagem de IoT, começando com o IoT Hub para suportar os cenários de ingestão de dados irá garantir que têm acesso instantânea para as capacidades de IoT completo assim que as suas necessidades empresariais e técnicas necessitem.

A tabela seguinte fornece detalhes sobre como as duas camadas do IoT Hub comparam com os Event Hubs quando estiver a avaliá-los para capacidades de IoT. Para obter mais informações sobre os escalões básicos e padrão de IoT Hub, consulte [como escolher o escalão de IoT Hub direita][lnk-scaling].

| Capacidade de IoT | Escalão padrão de IoT Hub | Escalão básico de IoT Hub | Event Hubs |
| --- | --- | --- | --- |
| Mensagens do dispositivo-nuvem | ![Marcar][1] | ![Marcar][1] | ![Marcar][1] |
| Protocolos: AMQP de HTTPS, AMQP, através de webSockets | ![Marcar][1] | ![Marcar][1] | ![Marcar][1] |
| Protocolos: MQTT, MQTT através de webSockets | ![Marcar][1] | ![Marcar][1] |  |
| Identidade por dispositivo | ![Marcar][1] | ![Marcar][1] |  |
| Carregamento de ficheiros a partir de dispositivos | ![Marcar][1] | ![Marcar][1] |  |
| Serviço de Aprovisionamento do Dispositivo | ![Marcar][1] | ![Marcar][1] |  |
| Mensagens cloud para dispositivo | ![Marcar][1] |  |  |
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
