---
title: Opções de dispositivo para a nuvem do IoT Hub do Azure | Microsoft Docs
description: Guia para programadores - orientações sobre quando utilizar mensagens do dispositivo para nuvem, propriedades comunicadas ou carregamento do ficheiro para comunicações de nuvem para o dispositivo.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: a1881b74ba3f4f66e9e47b24d2ee8c8c17c5f05d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634221"
---
# <a name="device-to-cloud-communications-guidance"></a>Orientações de comunicações do dispositivo-nuvem
Quando o envio de informações da aplicação de dispositivo para a solução de back-end, o IoT Hub expõe três opções:

* [Mensagens do dispositivo para nuvem] [ lnk-d2c] de telemetria séries de tempo e alertas.
* [Dispositivo duplo do comunicadas propriedades] [ lnk-twins] para as informações de estado do dispositivo como capacidades disponíveis, condições ou o estado de execução longa fluxos de trabalho de relatórios. Por exemplo, configuração e atualizações de software.
* [Carregamentos de ficheiros] [ lnk-fileupload] suportes de dados de ficheiros e lotes de telemetria grande foi carregado por dispositivos ligados intermitentemente ligados ou comprimido para poupar largura de banda.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Eis uma comparação detalhada das várias opções de comunicação do dispositivo para a nuvem.

|  | Mensagens do dispositivo para a cloud | Propriedades comunicado dispositivo duplo | Carregamentos de ficheiros |
| ---- | ------- | ---------- | ---- |
| Cenário | Série de tempo de telemetria e alertas. Por exemplo, os lotes de dados de sensores de 256 KB enviado a cada 5 minutos. | Capacidades disponíveis e condições. Por exemplo, o dispositivo conectividade modo atual, tais como a rede móvel ou Wi-Fi. A sincronizar os fluxos de trabalho de longa execução, tais como atualizações de software e configuração. | Ficheiros de suporte de dados. Lotes de telemetria (normalmente comprimido) grande. |
| Armazenamento e a obtenção | Armazenados temporariamente pelo IoT Hub, até a 7 dias. Apenas leitura sequencial. | Armazenados pelo IoT Hub no dispositivo duplo. Recuperável utilizando o [idioma de consulta do IoT Hub][lnk-query]. | Armazenados na conta de armazenamento do Azure fornecidos pelo utilizador. |
| Tamanho | Até 256 KB mensagens. | Tamanho máximo de propriedades comunicado é de 8 KB. | Tamanho de ficheiro máximo suportado pelo Blob Storage do Azure. |
| Frequência | Elevada. Para obter mais informações, consulte [limita do IoT Hub][lnk-quotas]. | Média. Para obter mais informações, consulte [limita do IoT Hub][lnk-quotas]. | Baixo. Para obter mais informações, consulte [limita do IoT Hub][lnk-quotas]. |
| Protocolo | Disponível em todos os protocolos. | Disponível através de MQTT ou AMQP. | Estão disponíveis ao utilizar qualquer protocolo, mas necessitar de HTTPS no dispositivo. |

Uma aplicação poderá ter de enviar informações tanto como um alerta ou uma série de tempo de telemetria e disponibilizá-lo no dispositivo duplo. Neste cenário, pode escolher uma das seguintes opções:

* A aplicação de dispositivo envia uma mensagem de dispositivo para a nuvem e reporta uma alteração de propriedade.
* O solução de back-end pode armazenar as informações nas tags do dispositivo duplo quando recebe a mensagem.

Uma vez que as mensagens do dispositivo para nuvem ativar um maior débito que atualizações ao dispositivo duplo, por vezes, é preferível evitar a atualizar o dispositivo duplo para cada mensagem dispositivo para a nuvem.


[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-fileupload]: iot-hub-devguide-file-upload.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
