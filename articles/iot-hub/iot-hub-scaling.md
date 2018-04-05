---
title: Dimensionamento IoT Hub do Azure | Microsoft Docs
description: Como dimensionar o seu IoT hub para suportar o débito de mensagem previsto e funcionalidades pretendidas. Inclui um resumo das opções de fragmentação e o débito suportado para cada camada.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.assetid: e7bd4968-db46-46cf-865d-9c944f683832
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2018
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3237ee41f8596ed3ce508857adf7dc29cee1ada
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>Escolher o escalão de IoT Hub à direita para a sua solução

Todas as soluções de IoT são diferente, pelo que o IoT Hub do Azure oferece várias opções baseadas no preço e escala. Este artigo destina-se para o ajudar a avaliar as suas necessidades de IoT Hub. Para obter informações sobre as camadas de IoT Hub de preços, consulte [IoT Hub preços](https://azure.microsoft.com/pricing/details/iot-hub). 

Para decidir que camada do IoT Hub é adequada para a sua solução, faça a mesmo duas questões:

**Quais as funcionalidades que planeia utilizar?**
IoT Hub do Azure oferece dois escalões básicos e padrão, que difere do número de funcionalidades que suportam. Se a sua solução de IoT baseada em torno da recolha de dados dos dispositivos e centralmente a analisar o escalão básico é provavelmente adequado para si. Se pretender utilizar configurações mais avançadas para controlar os dispositivos de IoT remotamente ou distribuir algumas das cargas de trabalho para os dispositivos de, em seguida, deve considerar o escalão standard. Para uma divisão de detalhado que funcionalidades estão incluídas em cada camada continuar a [escalões básicos e padrão](#basic-and-standard-tiers).

**Quantidade de dados planear mover diariamente?**
Cada escalão de IoT Hub está disponível em três tamanhos, com base em torno da quantidade débito de dados pode processar qualquer determinado dia. Estes tamanhos numerically são identificados como 1, 2 e 3. Por exemplo, cada unidade de um hub IoT de nível 1 pode processar mensagens de 400 thousand por dia, enquanto uma unidade de nível 3 pode processar milhões de 300. Para obter mais detalhes sobre as diretrizes de dados, continuar a [débito de mensagem](#message-throughput).

## <a name="basic-and-standard-tiers"></a>Escalões básicos e padrão

O escalão standard do IoT Hub permite que todas as funcionalidades e é necessário para as soluções de IoT que pretendem tornar utilizar as capacidades de comunicação bidirecional. O escalão básico permite um subconjunto das funcionalidades e destina-se as soluções de IoT que apenas necessitam de comunicações de sinalização uni unidirecional de dispositivos para a nuvem. Ambas as camadas oferecem as funcionalidades de segurança e autenticação mesmas.

Depois de criar o seu IoT hub pode atualizar a partir da camada básica para o escalão padrão sem interromper as operações existentes. Para obter mais informações, consulte [como atualizar o seu IoT hub](iot-hub-upgrade.md).

| Capacidade | Escalão Basic | Escalão Standard |
| ---------- | ---------- | ------------- |
| [Telemetria do dispositivo-nuvem](iot-hub-devguide-messaging.md) | Sim | Sim |
| [Identidade por dispositivo](iot-hub-devguide-identity-registry.md) | Sim | Sim |
| [Encaminhamento de mensagens](iot-hub-devguide-messages-read-custom.md) e [integração da grelha de eventos](iot-hub-event-grid.md) | Sim | Sim |
| [Protocolos HTTP, AMQP e MQTT](iot-hub-devguide-protocols.md) | Sim | Sim |
| [Serviço de aprovisionamento de dispositivos](../iot-dps/about-iot-dps.md) | Sim | Sim |
| [Monitorização e diagnóstico](iot-hub-monitor-resource-health.md) | Sim | Sim |
| [Mensagens da nuvem para dispositivo](iot-hub-devguide-c2d-guidance.md) |   | Sim |
| [Dispositivos duplos](iot-hub-devguide-device-twins.md) e [gestão de dispositivos](iot-hub-device-management-overview.md) |   | Sim |
| [Azure IoT Edge](../iot-edge/how-iot-edge-works.md) |   | Sim |

IoT Hub também oferece um escalão gratuito que destina-se para avaliação e teste. Tem todas as funcionalidades do escalão standard, mas allowances mensagens limitados. Não é possível atualizar do escalão gratuito para básico ou padrão. 

### <a name="iot-hub-rest-apis"></a>APIs REST do Hub IoT

A diferença no capacidades suportadas entre os escalões básicos e padrão de IoT Hub significa que algumas chamadas de API não funcionam com os hubs de escalão básicas. A tabela seguinte mostra as APIs estão disponíveis: 

| API | Escalão Basic | Escalão Standard |
| --- | ---------- | ------------- |
| [Eliminar o dispositivo](https://docs.microsoft.com/rest/api/iothub/deviceapi/deletedevice) | Sim | Sim |
| [Que o dispositivo](https://docs.microsoft.com/rest/api/iothub/deviceapi/getdevice) | Sim | Sim |
| [Obter estatísticas de registo](https://docs.microsoft.com/rest/api/iothub/deviceapi/getregistrystatistics) | Sim | Sim |
| [Obter estatísticas de serviços](https://docs.microsoft.com/rest/api/iothub/deviceapi/getservicestatistics) | Sim | Sim |
| [Colocar o dispositivo](https://docs.microsoft.com/rest/api/iothub/deviceapi/putdevice) | Sim | Sim |
| [Dispositivos de consulta](https://docs.microsoft.com/rest/api/iothub/deviceapi/querydevices) | Sim | Sim |
| [Criar o URI de SAS do carregamento de ficheiros](https://docs.microsoft.com/rest/api/iothub/httpruntime/createfileuploadsasuri) | Sim | Sim |
| [Receber notificações de dispositivo vinculado](https://docs.microsoft.com/rest/api/iothub/httpruntime/receivedeviceboundnotification) | Sim | Sim |
| [Enviar o evento de dispositivo](https://docs.microsoft.com/rest/api/iothub/httpruntime/senddeviceevent) | Sim | Sim |
| [Atualizar o estado de carregamento de ficheiros](https://docs.microsoft.com/rest/api/iothub/httpruntime/updatefileuploadstatus) | Sim | Sim |
| [Operação de dispositivo em massa](https://docs.microsoft.com/rest/api/iothub/deviceapi/bulkdeviceoperation) | Sim, exceto as capacidades de limite de IoT | Sim | 
| [Remover da fila de comando](https://docs.microsoft.com/rest/api/iothub/deviceapi/purgecommandqueue) |   | Sim |
| [Obter o dispositivo duplo](https://docs.microsoft.com/rest/api/iothub/devicetwinapi/getdevicetwin) |   | Sim |
| [Invocar o método de dispositivo](https://docs.microsoft.com/rest/api/iothub/devicetwinapi/invokedevicemethod) |   | Sim |
| [Atualizar o dispositivo duplo](https://docs.microsoft.com/rest/api/iothub/devicetwinapi/updatedevicetwin) |   | Sim | 
| [Abandonar o dispositivo vinculado notificação](https://docs.microsoft.com/rest/api/iothub/httpruntime/abandondeviceboundnotification) |   | Sim |
| [Dispositivo vinculado notificação](https://docs.microsoft.com/rest/api/iothub/httpruntime/completedeviceboundnotification) |   | Sim |
| [Cancelar a tarefa](https://docs.microsoft.com/rest/api/iothub/jobapi/canceljob) |   | Sim |
| [A tarefa de criação](https://docs.microsoft.com/rest/api/iothub/jobapi/createjob) |   | Sim |
| [Obter a tarefa](https://docs.microsoft.com/rest/api/iothub/jobapi/getjob) |   | Sim |
| [Tarefas de consulta](https://docs.microsoft.com/rest/api/iothub/jobapi/queryjobs) |   | Sim |

## <a name="message-throughput"></a>Débito de mensagem

A melhor forma de tamanho de uma solução de IoT Hub consiste em avaliar o tráfego numa base por unidade. Em particular, considere o débito de pico necessária para as seguintes categorias de operações:

* Mensagens do dispositivo para a cloud
* Mensagens da nuvem para dispositivo
* Operações de registo de identidade

O tráfego é medido numa base por unidade, não por hub. Uma instância do IoT Hub de nível 1 ou 2 pode ter até 200 unidades associadas à mesma. Uma instância do IoT Hub de nível 3 pode ter até 10 unidades. Depois de criar o seu IoT hub pode alterar o número de unidades ou mover entre o 1, 2 e 3 tamanhos dentro de um escalão específico sem interromper as operações existentes. Para obter mais informações, consulte [como atualizar o seu IoT Hub](iot-hub-upgrade.md).

Como um exemplo de capacidades de tráfego de cada camada, as mensagens do dispositivo para nuvem siga estas diretrizes de débito constante:

| Escalão | Débito constante | Velocidade de envio constante |
| --- | --- | --- |
| B1, S1 |Até 1111 KB por minuto por unidade<br/>(1,5 GB/dia/unidade) |Média de mensagens por minuto 278 por unidade<br/>(um máximo de 400.000 mensagens diário por unidade) |
| B2, S2 |Até 16 MB por minuto por unidade<br/>(22.8 GB/dia/unidade) |Média de mensagens por minuto 4,167 por unidade<br/>(milhões de 6 mensagens diário por unidade) |
| B3, S3 |Até 814 MB por minuto por unidade<br/>(1144.4 GB/dia/unidade) |Média de mensagens por minuto 208,333 por unidade<br/>(milhões de 300 mensagens diário por unidade) |

Para além desta informação de débito, consulte [quotas do IoT Hub e limitações] [ IoT Hub quotas and throttles] e estruturar a solução em conformidade.

### <a name="identity-registry-operation-throughput"></a>Débito de operação de registo de identidade
Operações de registo de identidade do IoT Hub são não deverá para ser o tempo de execução de operações, como estas são principalmente relacionadas com aprovisionamento de dispositivos.

Para números de desempenho de rajada específicos, consulte [quotas do IoT Hub e limitações][IoT Hub quotas and throttles].

## <a name="sharding"></a>Fragmentação
Embora um único IoT hub pode dimensionar para milhões de dispositivos, por vezes, a solução requer características de desempenho específica que não pode garantir que um único IoT hub. Nesse caso pode particionar os seus dispositivos entre vários os hubs IoT. Vários aos hubs IoT uniforme bursts de tráfego e obter o débito necessário ou taxas de operação que são necessárias.

## <a name="next-steps"></a>Passos seguintes

* Para obter informações adicionais sobre as capacidades do IoT Hub e detalhes de desempenho, consulte [IoT Hub preços] [preços ligação] ou [quotas do IoT Hub e limitações][IoT Hub quotas and throttles].
* Para alterar o escalão de IoT Hub, siga os passos no [atualizar o seu IoT hub](iot-hub-upgrade.md).

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
