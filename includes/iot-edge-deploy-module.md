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
ms.openlocfilehash: 92fb568bb1044df5be8c80372973743e2c0e3dcd
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2018
ms.locfileid: "52900995"
---
Uma das principais funcionalidades do Azure IoT Edge é conseguir implementar módulos nos seus dispositivos do IoT Edge a partir da cloud. Os módulos do IoT Edge são pacotes executáveis que são implementados como contentores. Nesta secção, vamos estará implantando um módulo previamente criado a partir da [seção de módulos do IoT Edge do Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules). Este módulo gera telemetria para o seu dispositivo simulado.

1. No portal do Azure, introduza `Simulated Temperature Sensor` para a pesquisa e abra o resultado do Marketplace.

   ![Sensor de temperatura simulada na pesquisa de portal do Azure](./media/iot-edge-deploy-module/search-for-temperature-sensor.png)

2. Na **subscrição** campo, selecione a subscrição com o IoT Hub que estiver a utilizar, se ainda não estiver.

3. Na **IoT Hub** campo, selecione o nome do IoT Hub que está a utilizar, se ainda não estiver.

4. Clique em **localizar dispositivo**, selecione o seu dispositivo IoT Edge (com o nome `myEdgeDevice`), em seguida, selecione **criar**.

5. Na **adicionar módulos** passo do assistente, clique nas **SimulatedTemperatureSensor** módulo para verificar as definições de configuração, clique em **guardar** e selecione **Próxima**.

6. Na **especificar rotas** passo do assistente, verifique se as rotas estão configuradas corretamente com a rota envia todas as mensagens de todos os módulos para o IoT Hub (`$upstream`). Caso contrário, adicione o seguinte código e, em seguida, selecione **Seguinte**.

   ```json
    {
    "routes": {
        "route": "FROM /messages/* INTO $upstream",
        "upstream": "FROM /messages/* INTO $upstream"
        }
    }
   ```

7. Na **implementação de revisão** passo do assistente, selecione **submeter**.

8. Regresse à página de detalhes do dispositivo e selecione **Atualizar**. Além do módulo de edgeAgent que foi criado quando o serviço é iniciado pela primeira vez, deverá ver outro módulo de tempo de execução chamado **edgeHub** e o **SimulatedTemperatureSensor** módulo listado.

   Pode demorar alguns minutos para que os novos módulos sejam apresentados. O dispositivo IoT Edge tem-se para obter novas informações de implementação a partir da cloud, iniciar os contentores e, em seguida, relatar o novo estado para o IoT Hub. 

   ![Vista SimulatedTemperatureSensor na lista de módulos implementados](./media/iot-edge-deploy-module/deployed-modules-marketplace-temp.png)
