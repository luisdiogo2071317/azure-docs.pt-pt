---
title: Esquema de dispositivo na solução de simulação de dispositivo - Azure | Documentos da Microsoft
description: Este artigo descreve o esquema JSON que define um dispositivo simulado na solução de simulação de dispositivos.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: a27624d4c7a0bde4b33aefe8d05881b743ce397d
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285839"
---
# <a name="understand-the-device-model-schema"></a>Compreender o esquema do modelo de dispositivo

Pode usar o acelerador de solução de simulação do dispositivo para gerar as cargas de teste para soluções que utilizam o IoT Hub. Ao implementar a solução de simulação do dispositivo, uma coleção de dispositivos simulados é aprovisionada automaticamente. Pode personalizar os dispositivos simulados existentes ou criar os seus próprios.

Este artigo descreve o esquema do modelo de dispositivo Especifica as capacidades e comportamento de um dispositivo simulado. O modelo do dispositivo é armazenado num ficheiro JSON.

Os artigos seguintes estão relacionados ao artigo atual:

* [Implementar o comportamento de modelo do dispositivo](iot-accelerators-device-simulation-device-behavior.md) descreve os ficheiros de JavaScript que utilizar para implementar o comportamento de um dispositivo simulado.
* [Criar um novo dispositivo simulado](iot-accelerators-remote-monitoring-test.md) coloca tudo isso e mostra-lhe como implementar um novo tipo de dispositivo simulado à sua solução.

Neste artigo, vai aprender a:

>[!div class="checklist"]
> * Utilizar um ficheiro JSON para definir um modelo de dispositivo simulado
> * Especifique as propriedades do dispositivo simulado
> * Especifique a telemetria que do dispositivo simulado envia
> * Especifique os métodos de cloud para o dispositivo, que o dispositivo responde a

[!INCLUDE [iot-accelerators-device-schema](../../includes/iot-accelerators-device-schema.md)]

## <a name="next-steps"></a>Passos Seguintes

Este artigo descreveu como criar seu próprio modelo personalizado de dispositivo simulado. Este artigo mostrou como para:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Utilizar um ficheiro JSON para definir um modelo de dispositivo simulado
> * Especifique as propriedades do dispositivo simulado
> * Especifique a telemetria que do dispositivo simulado envia
> * Especifique os métodos de cloud para o dispositivo, que o dispositivo responde a

Agora que aprendeu sobre o esquema JSON, o passo seguinte sugerido é saber como [implementar o comportamento do seu dispositivo simulado](iot-accelerators-device-simulation-device-behavior.md).

Para obter mais informações para desenvolvedores sobre a solução de simulação do dispositivo, consulte a [guia de referência do desenvolvedor](https://github.com/Azure/device-simulation-dotnet/wiki/Simulation-Service-Developer-Reference-Guide).
