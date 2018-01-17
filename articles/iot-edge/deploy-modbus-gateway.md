---
title: Implementar o Modbus no Azure IoT Edge | Microsoft Docs
description: "Permitir que os dispositivos que utilizam o Modbus TCP para comunicar com o Azure IoT Hub mediante a criação de um dispositivo de gateway do IoT Edge"
services: iot-Edge
documentationcenter: 
author: kgremban
manager: timlt
editor: chrisgmsft
ms.assetid: 
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: kgremban
ms.custom: 
ms.openlocfilehash: e239bde48c3da0d899e3c78bdd39f520c4128b95
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway---preview"></a>Ligar dispositivos Modbus TCP através de um gateway de dispositivo do IoT Edge – pré-visualização

Se quiser ligar dispositivos IoT que utilizam os protocolos Modbus TCP ou RTU a um hub IoT do Azure, utilize um dispositivo IoT Edge como gateway. O dispositivo de gateway lê os dados dos seus dispositivos Modbus e comunica-os para a cloud com um protocolo suportado. 

![Os dispositivos Modbus ligam-se ao Hub IoT através do gateway Edge.](./media/deploy-modbus-gateway/diagram.png)

Este artigo mostra como criar a sua própria imagem de contentor para um módulo do Modbus (também pode utilizar um exemplo pré-criado) e, em seguida, implementá-la no dispositivo IoT Edge que vai funcionar como o gateway. 

Este artigo pressupõe que está a utilizar o protocolo Modbus TCP. Para obter mais informações sobre como configurar o módulo para suportar o Modbus RTU, veja o projeto [Azure IoT Edge Modbus module](https://github.com/Azure/iot-edge-modbus) (Módulo Modbus do Azure IoT Edge) no Github. 

## <a name="prerequisites"></a>Pré-requisitos
* Um dispositivo Azure IoT Edge. Para obter instruções sobre como configurar um, veja [Deploy Azure IoT Edge on a simulated device in Windows](tutorial-simulate-device-windows.md) (Implementar o Azure IoT num dispositivo simulado no Windows) ou [Linux](tutorial-simulate-device-linux.md). 
* A cadeia de ligação da chave primária do dispositivo IoT Edge.
* Um dispositivo Modbus físico ou simulado que suporte o Modbus TCP.

## <a name="prepare-a-modbus-container"></a>Preparar um contentor do Modbus

Se quiser testar a funcionalidade do gateway do Modbus, a Microsoft disponibiliza-lhe um módulo de exemplo. Para utilizar o módulo de exemplo, aceda à secção [Executar a solução](#run-the-solution) e introduza o seguinte como o URI da Imagem: 

```URL
microsoft/azureiotedge-modbus-tcp:1.0-preview
```

Se quiser criar o seu próprio módulo e personalizá-lo para o seu ambiente, existe um projeto de código aberto no GitHub, o [Azure IoT Edge Modbus module](https://github.com/Azure/iot-edge-modbus) (Módulo do Modbus do Azure IoT Edge). Siga as orientações desse projeto para criar a sua própria imagem de contentor. Se criar a sua imagem, veja [Develop and deploy a C# IoT Edge module](tutorial-csharp-module.md) (Desenvolver e implementar um módulo do IoT Edge C#) para obter instruções sobre a publicação de imagens de contentores num registo e a implementação de módulos personalizados no seu dispositivo. 


## <a name="run-the-solution"></a>Executar a solução
1. No [portal do Azure](https://portal.azure.com/), aceda ao seu hub IoT.
2. Aceda a **IoT Edge (pré-visualização)** e selecione o seu dispositivo IoT Edge.
3. Selecione **Definir módulos**.
4. Adicione o módulo do Modbus:
   1. Selecione **Adicionar módulo do IoT Edge**.
   2. No campo **Nome**, introduza "modbus".
   3. No campo **Imagem**, introduza o URI da imagem do contentor de exemplo: `microsoft/azureiotedge-modbus-tcp:1.0-preview`.
   4. Selecione a caixa **Ativar** para atualizar as propriedades pretendidas do duplo do módulo.
   5. Copie o JSON seguinte para a caixa de texto. Altere o valor de **SlaveConnection** para o endereço IPv4 do seu dispositivo Modbus.

      ```JSON
      {  
        "properties.desired":{  
          "PublishInterval":"2000",
          "SlaveConfigs":{  
            "Slave01":{  
              "SlaveConnection":"<IPV4 address>",
              "HwId":"PowerMeter-0a:01:01:01:01:01",
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
9. No passo **Rever Modelo**, selecione **Submeter**. 
10. Regresse à página de detalhes do dispositivo e selecione **Atualizar**. Deverá ver o **modbus** novo em execução, juntamente com o runtime do IoT Edge.

## <a name="view-data"></a>Ver dados
Ver os dados que vêm do módulo modbus:
```cmd/sh
docker logs -f modbus
```

Também pode ver a telemetria que o dispositivo está a enviar com a [ferramenta do explorador do Hub IoT](https://github.com/azure/iothub-explorer). 

## <a name="next-steps"></a>Passos seguintes

- Para saber mais sobre como os dispositivos IoT Edge podem atuar como gateways, veja [Create an IoT Edge device that acts as a transparent gateway](how-to-create-transparent-gateway.md) (Criar um dispositivo IoT Edge que atue como gateway transparente)
- Para obter mais informações sobre como funcionam os módulos do IoT Edge, veja [Understand Azure IoT Edge modules](iot-edge-modules.md) (Compreender os módulos do Azure IoT Edge)