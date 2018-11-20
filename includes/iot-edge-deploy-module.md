---
title: incluir ficheiro
description: incluir ficheiro
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 10/14/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 536857a5fe3ec3cc037f21835a4152f93197bcb8
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977408"
---
Uma das principais funcionalidades do Azure IoT Edge é conseguir implementar módulos nos seus dispositivos do IoT Edge a partir da cloud. Os módulos do IoT Edge são pacotes executáveis que são implementados como contentores. Nesta secção, vai implementar um módulo que gera telemetria para o seu dispositivo simulado.

1. No portal do Azure, navegue para o seu hub IoT.

2. Aceda a **IoT Edge** sob **Gestão de Dispositivos Automáticos** e selecione o seu dispositivo IoT Edge.

3. Selecione **Definir Módulos**. É aberto um Assistente de três passos no portal, que fornece instruções para adicionar módulos, especificar rotas e revisão da implantação. 

4. Na **adicionar módulos** passo do assistente, localizar o **módulos de implementação** secção. Clique em **Add** , em seguida, selecione **módulo do IoT Edge**.

   ![Adicionar um novo módulo do IoT Edge](./media/iot-edge-deploy-module/add-module.png)

5. No campo **Nome**, introduza `tempSensor`.

6. No campo **URI da Imagem**, introduza `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0`.

7. Deixe as outras definições inalteradas e selecione **Guardar**.

   ![Guardar o módulo do IoT Edge depois de introduzir o nome e o URI da imagem](./media/iot-edge-deploy-module/name-image.png)

8. Na primeira etapa do assistente, selecione **seguinte**.

9. Na **especificar rotas** passo do assistente, deve ter uma rota envia todas as mensagens de todos os módulos para o IoT Hub. Caso contrário, adicione o seguinte código e, em seguida, selecione **Seguinte**.

   ```json
   {
       "routes": {
           "route": "FROM /messages/* INTO $upstream"
       }
   }
   ```

10. Na **implementação de revisão** passo do assistente, selecione **submeter**.

11. Regresse à página de detalhes do dispositivo e selecione **Atualizar**. Além do módulo edgeAgent que foi criado quando iniciou o serviço pela primeira vez, deve ver outro módulo do runtime chamado **edgeHub** e o módulo **tempSensor** listado.

   Pode demorar alguns minutos para que os novos módulos sejam apresentados. O dispositivo IoT Edge tem-se para obter novas informações de implementação a partir da cloud, iniciar os contentores e, em seguida, relatar o novo estado para o IoT Hub. 

   ![Vista de tempSensor na lista de módulos implementados](./media/iot-edge-deploy-module/deployed-modules.png)
