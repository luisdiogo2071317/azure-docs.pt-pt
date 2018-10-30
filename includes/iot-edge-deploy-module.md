---
title: incluir ficheiro
description: incluir ficheiro
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/14/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 983c65ba6e8b87f1dd66fcfdb50eac088ffab5d0
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49960193"
---
Uma das principais funcionalidades do Azure IoT Edge é conseguir implementar módulos nos seus dispositivos do IoT Edge a partir da cloud. Os módulos do IoT Edge são pacotes executáveis que são implementados como contentores. Nesta secção, vai implementar um módulo que gera telemetria para o seu dispositivo simulado.

1. No portal do Azure, navegue para o seu hub IoT.
1. Aceda a **IoT Edge** sob **Gestão de Dispositivos Automáticos** e selecione o seu dispositivo IoT Edge.
1. Selecione **Definir Módulos**.
1. Na secção **Módulos de Implementação** do passo **Adicionar Módulos**, clique em **Adicionar** e, em seguida, selecione **Módulo do IoT Edge**.
1. No campo **Nome**, introduza `tempSensor`.
1. No campo **URI da Imagem**, introduza `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0`.
1. Deixe as outras definições inalteradas e selecione **Guardar**.

   ![Guardar o módulo do IoT Edge depois de introduzir o nome e o URI da imagem](./media/iot-edge-deploy-module/name-image.png)

1. Novamente no passo **Adicionar módulos**, selecione **Seguinte**.
1. No passo **Especificar rotas**, deve ter uma rota predefinida que envia todas as mensagens de todos os módulos para o Hub IoT. Caso contrário, adicione o seguinte código e, em seguida, selecione **Seguinte**.

   ```json
   {
       "routes": {
           "route": "FROM /messages/* INTO $upstream"
       }
   }
   ```

1. No passo **Rever Implementação**, selecione **Submeter**.
1. Regresse à página de detalhes do dispositivo e selecione **Atualizar**. Além do módulo edgeAgent que foi criado quando iniciou o serviço pela primeira vez, deve ver outro módulo do runtime chamado **edgeHub** e o módulo **tempSensor** listado.

   ![Vista de tempSensor na lista de módulos implementados](./media/iot-edge-deploy-module/deployed-modules.png)
