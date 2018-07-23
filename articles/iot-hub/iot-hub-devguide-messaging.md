---
title: Compreender o sistema de mensagens do IoT Hub do Azure | Documentos da Microsoft
description: Guia do programador - dispositivo-para-cloud e na cloud para o dispositivo de mensagens com o IoT Hub. Inclui informações sobre formatos de mensagens e protocolos de comunicações suportados.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: b0667f820145f16c75a07ebe1849e20d2de36cc7
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185514"
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Dispositivo-para-cloud e as mensagens com o IoT Hub na cloud para dispositivo

Utilize o IoT Hub de mensagens para comunicar com os seus dispositivos por:

* Envio [dispositivo-para-cloud] [ lnk-d2c] mensagens dos seus dispositivos à sua solução de back-end.
* Envio [cloud-para-dispositivo] [ lnk-c2d] mensagens da solução de back-end para os seus dispositivos.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Propriedades de núcleo da funcionalidade de mensagens do IoT Hub são a fiabilidade e durabilidade de mensagens. Estas propriedades permitem maior resiliência às conectividade intermitente no lado do dispositivo e para carregar os picos no evento de processamento no lado da nuvem. Implementa o IoT Hub *, pelo menos, uma vez* garante a entrega de mensagens dispositivo-para-cloud e cloud-para-dispositivo.

Para obter uma introdução aos recursos do IoT Hub, veja a [descrição geral do serviço IoT Hub do Azure][lnk-iot-hub-overview].

## <a name="when-to-use-iot-hub-messaging"></a>Quando utilizar o IoT Hub de mensagens

Utilize mensagens de dispositivo-para-cloud para o envio de telemetria de série de tempo e os alertas da sua aplicação de dispositivo e mensagens de cloud-para-dispositivo para as notificações unidirecionais para a sua aplicação de dispositivo.

* Consulte a [documentação de orientação do dispositivo-para-cloud comunicação] [ lnk-d2c-guidance] se estiver em dúvida entre a utilização de mensagens do dispositivo para a cloud, as propriedades comunicadas ou carregamento de ficheiros.
* Consulte a [documentação de orientação do Cloud-para-dispositivo comunicação] [ lnk-c2d-guidance] se estiver em dúvida entre a utilização de mensagens da cloud para dispositivo, as propriedades pretendidas ou métodos diretos.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre o IoT Hub [mensagens dispositivo-para-cloud][lnk-d2c].
* Saiba mais sobre o IoT Hub [mensagens cloud-para-dispositivo][lnk-c2d].

[lnk-azure-iot]: ../iot-fundamentals/index.yml
[lnk-iot-hub-overview]: about-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md