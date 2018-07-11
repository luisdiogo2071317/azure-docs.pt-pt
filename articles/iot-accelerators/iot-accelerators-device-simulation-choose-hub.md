---
title: Utilizar um hub IoT com a solução de simulação do dispositivo - Azure | Documentos da Microsoft
description: Este artigo descreve como configurar o acelerador de solução de simulação do dispositivo para utilizar um IoT Hub.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 07/06/2018
ms.topic: conceptual
ms.openlocfilehash: ee96173ca5f36dee0f08c38e4b6e29da6fee804e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967548"
---
# <a name="use-an-existing-iot-hub-with-the-device-simulation-solution-accelerator"></a>Utilizar um hub IoT com o solution accelerator de simulação do dispositivo

Quando aprovisiona o acelerador de solução de simulação do dispositivo, pode optar por implementar um hub IoT no grupo de recursos do solution accelerator para utilizar na simulação.

Caso não pretenda implementar o IoT Hub opcional, tem de utilizar o seu próprio hub para qualquer simulações executar. Se optar por implementar o IoT Hub opcional, pode optar por utilizar este hub opcional ou o seu hub.

Se não tiver um hub IoT, pode sempre criar uma nova a partir da [portal do Azure](https://portal.azure.com).

Para utilizar um hub IoT já existente, terá de uma cadeia de ligação para o **iothubowner** partilhado a política de acesso. Pode obter esta cadeia de ligação a partir da [portal do Azure](https://portal.azure.com):

1. Na página de configuração do hub no portal, clique em **políticas de acesso partilhado**.
1. Clique em **iothubowner**.
1. Copie a cadeia de ligação primária ou secundária.

[![Obter cadeia de ligação](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-expanded.png#lightbox)

Utilize a cadeia de ligação que copiou quando configurar a simulação:

[![Configurar a simulação](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation-expanded.png#lightbox)

## <a name="next-steps"></a>Passos Seguintes

Este guia de procedimentos, aprendeu a utilizar o hub IoT existente numa simulação. Em seguida, pode querer saber como [configurar um modelo de dispositivo personalizado](iot-accelerators-device-simulation-custom-model.md) para uma simulação.
