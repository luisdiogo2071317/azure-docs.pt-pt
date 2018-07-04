---
title: incluir ficheiro
description: incluir ficheiro
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 06/27/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 9c562f1ce938e5f5d9371cbccf032c0eb1d67125
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37055035"
---
Uma das principais funcionalidades do Azure IoT Edge é conseguir implementar módulos nos seus dispositivos do IoT Edge a partir da cloud. Os módulos do IoT Edge são pacotes executáveis que são implementados como contentores. Nesta secção, vai implementar um módulo que gera telemetria para o seu dispositivo simulado. 

1. No portal do Azure, navegue para o seu hub IoT.
1. Aceda a **IoT Edge** e selecione o seu dispositivo IoT Edge.
1. Selecione **Definir Módulos**.
1. Na secção **Módulos de Implementação** da página, clique em **Adicionar** e, em seguida, selecione **Módulo do IoT Edge**.
1. No campo **Nome**, introduza `tempsensor`. 
1. No campo **URI da Imagem**, introduza `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0`. 
1. Deixe as outras definições inalteradas e selecione **Guardar**.

   ![Guardar o módulo do IoT Edge depois de introduzir o nome e o URI da imagem](./media/iot-edge-deploy-module/name-image.png)

1. Novamente no passo **Adicionar módulos**, selecione **Seguinte**.
1. No passo **Especificar rotas**, deve ter uma rota predefinida que envia todas as mensagens de todos os módulos para o Hub IoT. Caso contrário, adicione o seguinte código e, em seguida, selecione **Seguinte**.

   ```json
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```

1. No passo **Rever Implementação**, selecione **Submeter**.
1. Regresse à página de detalhes do dispositivo e selecione **Atualizar**. Além do módulo edgeAgent que foi criado quando iniciou o serviço pela primeira vez, deve ver outro módulo do runtime chamado **edgeHub** e o módulo **tempSensor** listado. 

   ![Vista de tempSensor na lista de módulos implementados](./media/iot-edge-deploy-module/deployed-modules.png)
