---
title: Opções de cloud para dispositivo IoT Hub do Azure | Documentos da Microsoft
description: Guia de desenvolvedor – documentação de orientação sobre quando utilizar métodos diretos, dispositivo duplo as propriedades pretendidas ou mensagens na cloud para o dispositivo para as comunicações da cloud para o dispositivo.
author: fsautomata
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: 2c2701b2ccfceef7f64078d13501aa9184fc87a3
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451634"
---
# <a name="cloud-to-device-communications-guidance"></a>Orientações de comunicações da cloud para dispositivo

O IoT Hub fornece três opções para aplicações de dispositivos para expor a funcionalidade para uma aplicação de back-end:

* [Métodos diretos](iot-hub-devguide-direct-methods.md) para comunicações que necessitam de confirmação imediata do resultado. Os métodos diretos são frequentemente utilizados para controlo interativo de dispositivos como ativar um fã.

* [Propriedades pretendidas duplo](iot-hub-devguide-device-twins.md) para comandos de execução longa se destina a colocar o dispositivo numa determinada o estado pretendido. Por exemplo, defina o intervalo de envio de telemetria para 30 minutos.

* [Mensagens da cloud para dispositivo](iot-hub-devguide-messages-c2d.md) para notificações unidirecionais para a aplicação de dispositivo.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Aqui está uma comparação detalhada das várias opções de comunicação de cloud-para-dispositivo.

|  | Métodos diretos | Propriedades de pretendidas do duplo | Mensagens da cloud para dispositivo |
| ---- | ------- | ---------- | ---- |
| Cenário | Comandos que necessitam de confirmação imediata, como ativar um fã. | Comandos de execução longa que se destina a colocar o dispositivo num determinado estado pretendido. Por exemplo, defina o intervalo de envio de telemetria para 30 minutos. | Obter notificações unidirecionais para a aplicação de dispositivo. |
| Fluxo de dados | Bidirecional. A aplicação de dispositivo pode responder ao método imediatamente. A solução de back-end recebe o resultado contextualmente para o pedido. | Unidirecional. A aplicação de dispositivo recebe uma notificação com a alteração de propriedade. | Unidirecional. A aplicação de dispositivo recebe a mensagem
| Durabilidade | Dispositivos desligados não estão a ser contactados. O back-end de solução é notificado de que o dispositivo não está ligado. | Valores de propriedade são mantidos no dispositivo duplo. Dispositivo será lê-lo no próximo restabelecimento de ligação. Valores de propriedade são recuperáveis com o [linguagem de consulta do IoT Hub](iot-hub-devguide-query-language.md). | As mensagens podem ser mantidas pelo IoT Hub até 48 horas. |
| Destinos | Dispositivo único usando **deviceId**, ou múltiplos dispositivos usando [tarefas](iot-hub-devguide-jobs.md). | Dispositivo único usando **deviceId**, ou múltiplos dispositivos usando [tarefas](iot-hub-devguide-jobs.md). | Dispositivo único por **deviceId**. |
| Tamanho | Tamanho de payload do método direto máximo é 128 KB. | Máximo pretendido do tamanho de propriedades é de 8 KB. | Até 64 KB mensagens. |
| Frequência | Elevada. Para obter mais informações, consulte [IoT Hub limita](iot-hub-devguide-quotas-throttling.md). | Média. Para obter mais informações, consulte [IoT Hub limita](iot-hub-devguide-quotas-throttling.md). | Baixo. Para obter mais informações, consulte [IoT Hub limita](iot-hub-devguide-quotas-throttling.md). |
| Protocolo | Disponível com MQTT ou AMQP. | Disponível com MQTT ou AMQP. | Disponível em todos os protocolos. Dispositivo tem de inquirir ao utilizar o HTTPS. |

Saiba como utilizar métodos diretos, as propriedades pretendidas e mensagens da cloud para dispositivo nos tutoriais seguintes:

* [Utilizar métodos diretos](quickstart-control-device-node.md)
* [Utilize as propriedades pretendidas para configurar dispositivos](tutorial-device-twins.md) 
* [Enviar mensagens da cloud para dispositivo](iot-hub-node-node-c2d.md)