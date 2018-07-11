---
title: Configurar um modelo de dispositivo personalizado - Azure | Documentos da Microsoft
description: Este artigo descreve como configurar um modelo de dispositivo personalizado do solution Accelerator de simulação do dispositivo.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 07/06/2018
ms.topic: conceptual
ms.openlocfilehash: 61a7c5314df541b4b44a286efe982f4bf93256e3
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967537"
---
# <a name="device-models"></a>Modelos de dispositivos

Quando configura uma simulação pode optar por utilizar um modelo do dispositivo existente com um predefinidos conjunto de sensores ou criar um modelo de dispositivo personalizado à sua escolha de sensores simulados. Sensores personalizados permitem-lhe mais modelar rigorosamente os seus dispositivos reais.

## <a name="pre-configured-device-models"></a>Modelos de dispositivos pré-configurado

O acelerador de solução de simulação do dispositivo fornece três modelos de dispositivos pré-configurados: Chillers elevadores e os autocarros.

Modelos de dispositivos pré-configurados tem vários sensores com comportamentos avançados definidos num arquivo JavaScript. Não é possível configurar esses comportamentos na IU da web.

A tabela seguinte mostra as configurações para cada modelo de dispositivo previamente configurado:

| Modelo do dispositivo  | Sensor           | Unidade  |
| ------------- | ---------------- | ----- |
| Chiller       | humidade         | %     |
|               | pressure         | psig  |
|               | temperatura      | F     |
| Elevator      | Piso            |       |
|               | Vibração        | mm    |
|               | Temperatura      | F     |
| Camião         | Latitude         |       |
|               | Longitude        |       |
|               | velocidade            | mph   |
|               | cargotemperature | F     |

## <a name="custom-device-models"></a>Modelos de dispositivos personalizados

Modelos de dispositivos personalizadas permitem-lhe modelar sensores que mais de perto a modelar seus próprios dispositivos. Um dispositivo personalizado pode ter até 10 sensores personalizados.

Quando seleciona o tipo de modelo de dispositivo personalizada, adicione sensores nova ao clicar em **+ adicionar sensor**.

[![Configurar sensores](./media/iot-accelerators-device-simulation-custom-model/configuresensors-inline.png)](./media/iot-accelerators-device-simulation-custom-model/configuresensors-expanded.png#lightbox)

Sensores personalizados têm as seguintes propriedades:

| Campo     | Descrição |
| --------- | ----------- |
| Nome do sensor | Um nome amigável para o sensor, tal como **temperatura** ou **velocidade**.  |
| Comportamento  | Comportamentos de ativar os dados de telemetria para variam de uma mensagem para a próxima para simular a dados do mundo real. **Incremento** aumenta o valor por um em cada mensagem enviada a partir do valor mínimo. Assim que o valor máximo é cumprido, em seguida, ele começará de novo novamente o valor mínimo. **Diminuição** se comporta da mesma forma como **incremento** mas contagens para baixo. O **Random** comportamento gera um valor aleatório entre os valores máximos e o valor mínimo. |
| Valor mínimo | O número mais baixo de seu intervalo aceitável. |
| Valor máximo | O número mais alto no seu intervalo aceitável. |
| Unidade      | A unidade de medida para o sensor, como ° F ou MPH. |

## <a name="next-steps"></a>Passos Seguintes

Este guia de procedimentos, já aprendeu a configurar um modelo de dispositivo personalizado para uma simulação. Em seguida, pode querer explorar alguns dos outros [Aceleradores de solução de IoT](about-iot-accelerators.md).