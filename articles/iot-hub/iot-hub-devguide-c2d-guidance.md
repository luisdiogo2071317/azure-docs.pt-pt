---
title: Opções de cloud para dispositivo IoT Hub do Azure | Documentos da Microsoft
description: Guia de desenvolvedor – documentação de orientação sobre quando utilizar métodos diretos, dispositivo duplo as propriedades pretendidas ou mensagens na cloud para o dispositivo para as comunicações da cloud para o dispositivo.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: 2cc9bd39371741caaa3ae025df494e225dc754b0
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187041"
---
# <a name="cloud-to-device-communications-guidance"></a>Orientações de comunicações da cloud para dispositivo
O IoT Hub fornece três opções para aplicações de dispositivos para expor a funcionalidade para uma aplicação de back-end:

* [Métodos diretos] [ lnk-methods] para comunicações que necessitam de confirmação imediata do resultado. Os métodos diretos são frequentemente utilizados para controlo interativo de dispositivos como ativar um fã.
* [Duplo propriedades pretendidas] [ lnk-twins] para comandos de execução longa se destina a colocar o dispositivo numa determinada o estado pretendido. Por exemplo, defina o intervalo de envio de telemetria para 30 minutos.
* [Mensagens cloud para o dispositivo] [ lnk-c2d] para notificações unidirecionais para a aplicação de dispositivo.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Aqui está uma comparação detalhada das várias opções de comunicação de cloud-para-dispositivo.

|  | Métodos diretos | Propriedades de pretendidas do duplo | Mensagens da cloud para dispositivo |
| ---- | ------- | ---------- | ---- |
| Cenário | Comandos que necessitam de confirmação imediata, como ativar um fã. | Comandos de execução longa que se destina a colocar o dispositivo num determinado estado pretendido. Por exemplo, defina o intervalo de envio de telemetria para 30 minutos. | Obter notificações unidirecionais para a aplicação de dispositivo. |
| Fluxo de dados | Bidirecional. A aplicação de dispositivo pode responder ao método imediatamente. A solução de back-end recebe o resultado contextualmente para o pedido. | Unidirecional. A aplicação de dispositivo recebe uma notificação com a alteração de propriedade. | Unidirecional. A aplicação de dispositivo recebe a mensagem
| Durabilidade | Dispositivos desligados não estão a ser contactados. O back-end de solução é notificado de que o dispositivo não está ligado. | Valores de propriedade são mantidos no dispositivo duplo. Dispositivo será lê-lo no próximo restabelecimento de ligação. Valores de propriedade são recuperáveis com o [linguagem de consulta do IoT Hub][lnk-query]. | As mensagens podem ser mantidas pelo IoT Hub até 48 horas. |
| Destinos | Dispositivo único usando **deviceId**, ou múltiplos dispositivos usando [trabalhos][lnk-jobs]. | Dispositivo único usando **deviceId**, ou múltiplos dispositivos usando [trabalhos][lnk-jobs]. | Dispositivo único por **deviceId**. |
| Tamanho | Tamanho de payload do método direto máximo é 128 KB. | Máximo pretendido do tamanho de propriedades é de 8 KB. | Até 64 KB mensagens. |
| Frequência | Elevada. Para obter mais informações, consulte [IoT Hub limita][lnk-quotas]. | Média. Para obter mais informações, consulte [IoT Hub limita][lnk-quotas]. | Baixo. Para obter mais informações, consulte [IoT Hub limita][lnk-quotas]. |
| Protocolo | Disponível com MQTT ou AMQP. | Disponível com MQTT ou AMQP. | Disponível em todos os protocolos. Dispositivo tem de inquirir ao utilizar o HTTPS. |

Saiba como utilizar métodos diretos, as propriedades pretendidas e mensagens da cloud para dispositivo nos tutoriais seguintes:

* [Utilizar métodos diretos][lnk-methods-tutorial], para métodos diretos;
* [Utilize as propriedades pretendidas para configurar dispositivos][lnk-twin-properties], para o dispositivo duplo propriedades; pretendidas 
* [Enviar mensagens da cloud para o dispositivo][lnk-c2d-tutorial], para mensagens na cloud para o dispositivo.

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-methods-tutorial]: quickstart-control-device-node.md
[lnk-twin-properties]: tutorial-device-twins.md
[lnk-c2d-tutorial]: iot-hub-node-node-c2d.md
