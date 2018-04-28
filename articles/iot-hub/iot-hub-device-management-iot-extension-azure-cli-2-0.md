---
title: Azure gestão de dispositivos de IoT com a extensão do IoT do Azure CLI 2.0 | Microsoft Docs
description: Utilize a extensão de IoT para a ferramenta Azure CLI 2.0 para gestão de dispositivos do IoT Hub do Azure, com os métodos diretos e as opções de gestão do duplo propriedades pretendido.
services: iot-hub
documentationcenter: ''
author: chrissie926
manager: timlt
tags: ''
keywords: gestão de dispositivos do hub iot do azure, gestão de dispositivos iot do Azure, a gestão de dispositivos do iot hub e iot de gestão de dispositivos
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2018
ms.author: menchi
ms.openlocfilehash: 1bf2c10c5267157f6d74962c00d587a6956fc574
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="use-the-iot-extension-for-azure-cli-20-for-azure-iot-hub-device-management"></a>Utilizar a extensão de IoT para Azure CLI 2.0 para gestão de dispositivos do IoT Hub do Azure

![Diagrama de ponto a ponto](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[A extensão de IoT para Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) é uma origem de abrir nova extensão de IoT que adiciona às capacidades do [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest). Azure CLI 2.0 inclui comandos para interagir com pontos finais de gestão e o Gestor de recursos do Azure. Por exemplo, pode utilizar o Azure CLI 2.0 para criar uma VM do Azure ou um IoT hub. Uma extensão CLI permite que um serviço do Azure aumentar a CLI do Azure dá ao que aceder às funcionalidades adicionais do específicos do serviço. A extensão de IoT dá-os programadores do IoT acesso de linha de comandos para capacidades de todos os IoT Hub, o limite de IoT e serviço de aprovisionamento de dispositivos IoT Hub.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Opção de gestão          | Tarefa                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Métodos diretos             | Se um dispositivo agir como iniciar ou parar o envio de mensagens ou iniciar o dispositivo.                                        |
| Propriedades de duplo pretendida    | Colocar um dispositivo em determinados Estados, como a definição de um LED como verde ou definir o intervalo de envio de telemetria e 30 minutos.         |
| Duplo comunicadas propriedades   | Obter o estado de um dispositivo reportado. Por exemplo, o dispositivo comunica que o LED é blinking agora.                                    |
| Etiquetas de duplo                  | Armazenar metadados específicos do dispositivo na nuvem. Por exemplo, a localização de implementação de uma máquina de distribuidores automáticos.                         |
| Consultas do dispositivo duplo        | Consulta todos os dispositivos duplos, para obter as condições arbitrários, como identificar os dispositivos que estão disponíveis para utilização. |

Para obter mais explicação sobre as diferenças e as orientações sobre como utilizar estas opções, consulte [orientações de comunicação do dispositivo para nuvem](iot-hub-devguide-d2c-guidance.md) e [orientações de comunicação de nuvem para o dispositivo](iot-hub-devguide-c2d-guidance.md).

Os dispositivos duplos são documentos JSON que armazenam informações de estado dos dispositivos (metadados, configurações e condições). IoT Hub mantém um dispositivo duplo para cada dispositivo que se liga ao mesmo. Para obter mais informações sobre dispositivos duplos, consulte [começar a utilizar dispositivos duplos](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>O que irá aprender

Aprender a utilizar a extensão de IoT para Azure CLI 2.0 com várias opções de gestão no computador de desenvolvimento.

## <a name="what-you-do"></a>O que fazer

Executar o Azure CLI 2.0 e a extensão de IoT para Azure CLI 2.0 com várias opções de gestão.

## <a name="what-you-need"></a>Do que precisa

- Tutorial [configurar o seu dispositivo](iot-hub-raspberry-pi-kit-node-get-started.md) concluir que inclui os seguintes requisitos:
  - Uma subscrição ativa do Azure.
  - Um hub IoT do Azure na sua subscrição.
  - Uma aplicação de cliente que envia mensagens para o seu hub IoT do Azure.

- Certifique-se de que o dispositivo está em execução com a aplicação de cliente durante este tutorial.

- [Python 2.7x ou Python 3.x](https://www.python.org/downloads/)

- Instale a CLI do Azure 2.0. Uma forma simples de instalar no Windows é transferir e instalar o [MSI](https://aka.ms/InstallAzureCliWindows). Também pode seguir as instruções de instalação no [Microsoft Docs](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ao configurar o Azure CLI 2.0 no seu ambiente. A versão mínima da CLI 2.0 do Azure tem de ser 2.0.24 ou superior. Utilize `az –version` para validar. 

- Instale a extensão de IoT. A forma mais simples consiste em executar `az extension add --name azure-cli-iot-ext`. [O ficheiro Leia-me da extensão de IoT](https://github.com/Azure/azure-iot-cli-extension/blob/master/README.md) descreve várias formas de instalar a extensão.


## <a name="log-in-to-your-azure-account"></a>Iniciar sessão na sua conta do Azure

Inicie sessão sua conta do Azure executando o seguinte comando:

```bash
az login
```

## <a name="direct-methods"></a>Métodos diretos

```bash
az iot hub invoke-device-method --device-id <your device id> --hub-name <your hub name> --method-name <the method name> --method-payload <the method payload>
```

## <a name="device-twin-desired-properties"></a>Propriedades do dispositivo duplo pretendida

Definir um intervalo de propriedade pretendido = 3000 executando o seguinte comando:

```bash
az iot hub device-twin update -n <your hub name> -d <your device id> --set properties.desired.interval = 3000
```

Esta propriedade pode ser lidos a partir do dispositivo.

## <a name="device-twin-reported-properties"></a>Dispositivo duplo comunicadas propriedades

Obter as propriedades do dispositivo comunicadas executando o seguinte comando:

```bash
az iot hub device-twin update -n <your hub name> -d <your device id> --set properties.reported.interval = 3000
```

Uma das propriedades é $metadata. $lastUpdated que mostra a hora da última este dispositivo envia ou recebe uma mensagem.

## <a name="device-twin-tags"></a>Etiquetas do dispositivo duplo

Apresente as propriedades do dispositivo e etiquetas, executando o seguinte comando:

```bash
az iot hub device-twin show --hub-name <your hub name> --device-id <your device id>
```

Adicionar uma função de campo = temperatura e humidade para o dispositivo, executando o seguinte comando:

```bash
az iot hub device-twin update --hub-name <your hub name> --device-id <your device id> --set tags = '{"role":"temperature&humidity"}}'
```

## <a name="device-twin-queries"></a>Consultas do dispositivo duplo

Consultar os dispositivos com uma etiqueta de função = 'temperatura e humidade' executando o seguinte comando:

```bash
az iot hub query --hub-name <your hub name> --query-command "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Consultar todos os dispositivos, exceto aqueles com uma etiqueta de função = 'temperatura e humidade' executando o seguinte comando:

```bash
az iot hub query --hub-name <your hub name> --query-command "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Passos Seguintes

Aprendeu como monitorizar as mensagens do dispositivo para nuvem e enviar mensagens da nuvem para o dispositivo entre os dispositivos de IoT e o IoT Hub do Azure.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
