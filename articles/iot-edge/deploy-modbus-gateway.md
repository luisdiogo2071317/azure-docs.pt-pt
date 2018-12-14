---
title: Traduzir protocolos modbus com gateways - Azure IoT Edge | Documentos da Microsoft
description: Permitir que os dispositivos que utilizam o Modbus TCP para comunicar com o Azure IoT Hub mediante a criação de um dispositivo de gateway do IoT Edge
author: kgremban
manager: philmea
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 0b94a57f04d9e4b5884162dcd4cfd89d6b4bf879
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53341036"
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway"></a>Ligar dispositivos Modbus TCP através de um gateway de dispositivo do IoT Edge

Se quiser ligar dispositivos IoT que utilizam os protocolos Modbus TCP ou RTU a um hub IoT do Azure, utilize um dispositivo IoT Edge como gateway. O dispositivo de gateway lê os dados dos seus dispositivos Modbus e comunica-os para a cloud com um protocolo suportado.

![Os dispositivos Modbus ligam-se ao Hub IoT através do gateway Edge.](./media/deploy-modbus-gateway/diagram.png)

Este artigo mostra como criar a sua própria imagem de contentor para um módulo do Modbus (também pode utilizar um exemplo pré-criado) e, em seguida, implementá-la no dispositivo IoT Edge que vai funcionar como o gateway.

Este artigo pressupõe que está a utilizar o protocolo Modbus TCP. Para obter mais informações sobre como configurar o módulo para suportar o Modbus RTU, veja a [módulo do Azure IoT Edge Modbus](https://github.com/Azure/iot-edge-modbus) projeto no GitHub.

## <a name="prerequisites"></a>Pré-requisitos
* Um dispositivo Azure IoT Edge. Para obter instruções sobre como configurar um, veja [Deploy Azure IoT Edge on a simulated device in Windows](quickstart.md) (Implementar o Azure IoT num dispositivo simulado no Windows) ou [Linux](quickstart-linux.md).
* A cadeia de ligação da chave primária do dispositivo IoT Edge.
* Um dispositivo Modbus físico ou simulado que suporte o Modbus TCP.

## <a name="prepare-a-modbus-container"></a>Preparar um contentor do Modbus

Se quiser testar a funcionalidade do gateway do Modbus, a Microsoft disponibiliza-lhe um módulo de exemplo. Para utilizar o módulo de exemplo, aceda à secção [Executar a solução](#run-the-solution) e introduza o seguinte como o URI da Imagem:

```URL
mcr.microsoft.com/azureiotedge/modbus:1.0
```

Se quiser criar seu próprio módulo e personalizá-lo para o seu ambiente, existe uma código-fonte aberto [módulo do Azure IoT Edge Modbus](https://github.com/Azure/iot-edge-modbus) projeto no GitHub. Siga as orientações desse projeto para criar a sua própria imagem de contentor. Se criar a sua imagem, veja [Develop and deploy a C# IoT Edge module](tutorial-csharp-module.md) (Desenvolver e implementar um módulo do IoT Edge C#) para obter instruções sobre a publicação de imagens de contentores num registo e a implementação de módulos personalizados no seu dispositivo.


## <a name="run-the-solution"></a>Executar a solução
1. No [portal do Azure](https://portal.azure.com/), aceda ao seu hub IoT.
2. Aceda a **IoT Edge** e clique no seu dispositivo IoT Edge.
3. Selecione **Definir módulos**.
4. Adicione o módulo do Modbus:
   1. Clique em **Add** e selecione **módulo do IoT Edge**.
   2. No campo **Nome**, introduza "modbus".
   3. No campo **Imagem**, introduza o URI da imagem do contentor de exemplo: `mcr.microsoft.com/azureiotedge/modbus:1.0`.
   4. Selecione a caixa **Ativar** para atualizar as propriedades pretendidas do duplo do módulo.
   5. Copie o JSON seguinte para a caixa de texto. Altere o valor de **SlaveConnection** para o endereço IPv4 do seu dispositivo Modbus.

      ```JSON
      {
        "properties.desired":{
          "PublishInterval":"2000",
          "SlaveConfigs":{
            "Slave01":{
              "SlaveConnection":"<IPV4 address>","HwId":"PowerMeter-0a:01:01:01:01:01",
              "Operations":{
                "Op01":{
                  "PollingInterval": "1000",
                  "UnitId":"1",
                  "StartAddress":"400001",
                  "Count":"2",
                  "DisplayName":"Voltage"
                }
              }
            }
          }
        }
      }
      ```

   6. Selecione **Guardar**.
5. Novamente no passo **Adicionar módulos**, selecione **Seguinte**.
7. No passo **Especificar Rotas**, copie o JSON seguinte para a caixa de texto. Esta rota envia todas as mensagens recolhidas pelo módulo Modbus para o IoT Hub. Nesta rota, ''modbusOutput'' é o ponto final que o módulo Modbus utiliza para produzir dados e “upstream” é um destino especial que diz ao Edge Hub para enviar mensagens para o Hub IoT.
   ```JSON
   {
    "routes": {
      "modbusToIoTHub":"FROM /messages/modules/modbus/outputs/modbusOutput INTO $upstream"
    }
   }
   ```

8. Selecione **Seguinte**.
9. No passo **Rever Implementação**, selecione **Submeter**.
10. Regresse à página de detalhes do dispositivo e selecione **Atualizar**. Deverá ver a nova **modbus** módulo em execução, juntamente com o runtime do IoT Edge.

## <a name="view-data"></a>Ver dados
Ver os dados que vêm do módulo modbus:
```cmd/sh
docker logs -f modbus
```

Também pode ver a telemetria que o dispositivo está a enviar ao utilizar o [extensão de Kit de ferramentas do Azure IoT Hub para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (anteriormente conhecido como extensão do Kit de ferramentas do Azure IoT).

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre como os dispositivos IoT Edge podem atuar como gateways, veja [Create an IoT Edge device that acts as a transparent gateway](./how-to-create-transparent-gateway.md) (Criar um dispositivo IoT Edge que atue como gateway transparente)
- Para obter mais informações sobre como funcionam os módulos do IoT Edge, veja [Understand Azure IoT Edge modules](iot-edge-modules.md) (Compreender os módulos do Azure IoT Edge)
