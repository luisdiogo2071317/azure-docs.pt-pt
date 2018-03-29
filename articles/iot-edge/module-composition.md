---
title: Composição do módulo de limite de IoT do Azure | Microsoft Docs
description: Saiba o que fica em módulos de limite de IoT do Azure e como pode ser reutilizadas
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/23/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 7df566ced755e1e817b3107dac8f17e9f6e9b8e4
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="understand-how-iot-edge-modules-can-be-used-configured-and-reused---preview"></a>Compreender como módulos de IoT limite podem ser utilizados, configurado e reutilizada - pré-visualização

Cada dispositivo de limite de IoT é executado, pelo menos, dois módulos: $edgeAgent e $edgeHub, constituem o tempo de execução do limite de IoT. Para além das duas padrão, qualquer dispositivo de limite de IoT pode executar vários módulos para efetuar qualquer número de processos. Quando implementar em simultâneo todos os estes módulos num dispositivo, terá de uma forma de declarar os módulos são incluídos como eles interagem entre si. 

O *o manifesto de implementação* é um documento JSON que descreve:

* Os módulos de IoT contorno tem de ser implementado, juntamente com as opções de criação e gestão.
* A configuração do hub de limite, o que inclui a forma como o fluxo de mensagens entre módulos e, eventualmente, ao IoT Hub.
* Opcionalmente, os valores para definir nas propriedades pretendidas duplos módulo, para configurar as aplicações de módulo individuais.

Todos os dispositivos de IoT contorno tem de ser configurado com um manifesto de implementação. Os relatórios de um tempo de execução de limite de IoT recentemente instalado um código de erro até configurado com um manifesto válido. 

Os tutoriais de limite de IoT do Azure, criar um manifesto de implementação acedendo através de um assistente no portal do Azure IoT Edge. Também pode aplicar um manifesto de implementação utilizando através de programação REST ou o SDK do serviço do IoT Hub. Consulte [implementar e monitorizar] [ lnk-deploy] para obter mais informações sobre implementações de limite de IoT.

## <a name="create-a-deployment-manifest"></a>Crie um manifesto de implementação

Um nível elevado, o manifesto de implementação configura propriedades pretendido de um duplo de módulo de módulos de limite de IoT implementados num dispositivo de limite de IoT. Dois destes módulos são sempre presente: o agente de limite e o hub de limite.

Um manifesto de implementação que contém apenas o tempo de execução do limite de IoT (agente e hub) é válido.

O manifesto segue esta estrutura:

```json
{
    "moduleContent": {
        "$edgeAgent": {
            "properties.desired": {
                // desired properties of the Edge agent
                // includes the image URIs of all modules
            }
        },
        "$edgeHub": {
            "properties.desired": {
                // desired properties of the Edge hub
                // includes the routing information between modules, and to IoT Hub
            }
        },
        "{module1}": {  // optional
            "properties.desired": {
                // desired properties of module with id {module1}
            }
        },
        "{module2}": {  // optional
            ...
        },
        ...
    }
}
```

## <a name="configure-modules"></a>Configurar módulos

Para além de estabelecer as propriedades de quaisquer módulos de que pretende implementar pretendidas, terá de informar o tempo de execução do IoT Edge como instalá-los. As informações de configuração e gestão de todos os módulos ficar no interior do **$edgeAgent** pretendido propriedades. Estas informações incluem os parâmetros de configuração para o próprio agente de limite. 

Para obter uma lista completa das propriedades que podem ou tem de ser incluída, consulte [propriedades do agente de limite e Edge hub](module-edgeagent-edgehub.md).

As propriedades de $edgeAgent siga esta estrutura:

```json
"$edgeAgent": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
        },
        "systemModules": {
            "edgeAgent": {
                // configuration and management details
            },
            "edgeHub": {
                // configuration and management details
            }
        },
        "modules": {
            "{module1}": { //optional
                // configuration and management details
            },
            "{module2}": { // optional
                // configuration and management details
            }
        }
    }
},
```

## <a name="declare-routes"></a>Declarar as rotas

Hub de limite fornece uma forma de forma declarativa encaminhar mensagens entre módulos e entre módulos e IoT Hub. O hub de limite gere todas as comunicações, pelo que as informações de rota ficar no interior do **$edgeHub** pretendido propriedades. Pode ter várias rotas dentro da mesma implementação.

Rotas declaradas no **$edgeHub** pretendido propriedades com a seguinte sintaxe:

```json
"$edgeHub": {
    "properties.desired": {
        "routes": {
            "{route1}": "FROM <source> WHERE <condition> INTO <sink>",
            "{route2}": "FROM <source> WHERE <condition> INTO <sink>"
        },
    }
}
```

Cada rota necessita de uma origem e um receptor, mas a condição é uma informação opcional que pode utilizar para filtrar mensagens. 


### <a name="source"></a>Origem
A origem Especifica onde provenientes as mensagens. Pode ser qualquer um dos seguintes valores:

| Origem | Descrição |
| ------ | ----------- |
| `/*` | Todas as mensagens do dispositivo para a nuvem de qualquer dispositivo ou o módulo |
| `/messages/*` | Qualquer mensagem de dispositivo para nuvem enviada por um dispositivo ou um módulo através de alguns ou nenhum resultado |
| `/messages/modules/*` | Qualquer mensagem de dispositivo para nuvem enviada por um módulo através de alguns ou nenhum resultado |
| `/messages/modules/{moduleId}/*` | Qualquer mensagem de dispositivo para nuvem enviada por {moduleId} sem saída |
| `/messages/modules/{moduleId}/outputs/*` | Qualquer mensagem de dispositivo para nuvem enviada por {moduleId}, com algumas saídas de |
| `/messages/modules/{moduleId}/outputs/{output}` | Qualquer mensagem de dispositivo para nuvem enviada através de {moduleId} {saída} |

### <a name="condition"></a>Condição
A condição é opcional numa declaração rota. Se pretender passar todas as mensagens do sink de para a origem, basta deixar o **onde** cláusula completamente. Ou pode utilizar o [idioma de consulta do IoT Hub] [ lnk-iothub-query] para filtrar para determinados tipos de mensagem que satisfaçam a condição ou mensagens.

As mensagens que transmita entre os módulos no limite de IoT são formatadas o mesmo que as mensagens que transmita entre os seus dispositivos e IoT Hub do Azure. Todas as mensagens são formatadas como JSON e ter **systemProperties**, **appProperties**, e **corpo** parâmetros. 

Pode criar consultas em torno de todos os três parâmetros com a seguinte sintaxe: 

* Propriedades do sistema: `$<propertyName>` ou `{$<propertyName>}`
* Propriedades da aplicação: `<propertyName>`
* Propriedades de corpo: `$body.<propertyName>` 

Para obter exemplos sobre como criar consultas para as propriedades da mensagem, consulte [rotas expressões de consulta de mensagem de dispositivo para nuvem](../iot-hub/iot-hub-devguide-query-language.md#device-to-cloud-message-routes-query-expressions).

Um exemplo que é específico para limite de IoT é quando pretender filtrar mensagens em fila que chegaram a um dispositivo de gateway a partir de um dispositivo de folha. Mensagens provenientes de módulos contém uma propriedade de sistema chamada **connectionModuleId**. Por isso, se pretender encaminhar mensagens a partir de dispositivos de folha diretamente ao IoT Hub, utilize a rota seguinte para excluir mensagens do módulo:

```sql
FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO $upstream
```

### <a name="sink"></a>Sink
O sink define onde as mensagens são enviadas. Pode ser qualquer um dos seguintes valores:

| Sink | Descrição |
| ---- | ----------- |
| `$upstream` | Enviar a mensagem ao IoT Hub |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Enviar a mensagem de entrada `{input}` do módulo `{moduleId}` |

É importante ter em atenção que Edge hub fornece garantias em-menos-uma vez, que significa que as mensagens são armazenadas localmente no caso de uma rota não é possível entregar a mensagem à respetiva dependente, por exemplo, o hub Edge não é possível ligar ao IoT Hub ou o módulo de destino não está ligado.

Edge hub armazena as mensagens até à hora especificada no `storeAndForwardConfiguration.timeToLiveSecs` propriedade o [Edge hub pretendido propriedades](module-edgeagent-edgehub.md).

## <a name="define-or-update-desired-properties"></a>Definir ou atualizar propriedades pretendidas 

O manifesto de implementação pode especificar propriedades pretendidas para o duplo de módulo de cada módulo implementado no dispositivo de limite de IoT. Quando as propriedades pretendidas são especificadas no manifesto de implementação, podem substituem quaisquer propriedades pretendidas atualmente no duplo módulo.

Se não especificar propriedades de pretendido de um duplo de módulo no manifesto de implementação, o IoT Hub não irá modificar o duplo de módulo de qualquer forma e conseguirá definir as propriedades de pretendido através de programação.

Os mesmos mecanismos que lhe permitem modificar dispositivos duplos são utilizados para modificar duplos do módulo. Consulte o [guia para programadores do dispositivo duplo](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins) para obter mais informações.   

## <a name="deployment-manifest-example"></a>Exemplo de manifesto de implementação

Este exemplo de um documento JSON manifesto de implementação.

```json
{
"moduleContent": {
    "$edgeAgent": {
        "properties.desired": {
            "schemaVersion": "1.0",
            "runtime": {
                "type": "docker",
                "settings": {
                    "minDockerVersion": "v1.25",
                    "loggingOptions": ""
                }
            },
            "systemModules": {
                "edgeAgent": {
                    "type": "docker",
                    "settings": {
                    "image": "microsoft/azureiotedge-agent:1.0-preview",
                    "createOptions": ""
                    }
                },
                "edgeHub": {
                    "type": "docker",
                    "status": "running",
                    "restartPolicy": "always",
                    "settings": {
                    "image": "microsoft/azureiotedge-hub:1.0-preview",
                    "createOptions": ""
                    }
                }
            },
            "modules": {
                "tempSensor": {
                    "version": "1.0",
                    "type": "docker",
                    "status": "running",
                    "restartPolicy": "always",
                    "settings": {
                    "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
                    "createOptions": "{}"
                    }
                },
                "filtermodule": {
                    "version": "1.0",
                    "type": "docker",
                    "status": "running",
                    "restartPolicy": "always",
                    "settings": {
                    "image": "myacr.azurecr.io/filtermodule:latest",
                    "createOptions": "{}"
                    }
                }
            }
        }
    },
    "$edgeHub": {
        "properties.desired": {
            "schemaVersion": "1.0",
            "routes": {
                "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
                "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
            },
            "storeAndForwardConfiguration": {
                "timeToLiveSecs": 10
            }
        }
    }
}
}
```

## <a name="next-steps"></a>Passos Seguintes

* Para obter uma lista completa das propriedades que podem ou têm de ser incluídos no $edgeAgent e $edgeHub, consulte [propriedades do agente de limite e Edge hub](module-edgeagent-edgehub.md).

* Agora que já sabe como são utilizados os módulos de IoT Edge, [compreender os requisitos e ferramentas para o desenvolvimento de módulos de limite de IoT][lnk-module-dev].

[lnk-deploy]: module-deployment-monitoring.md
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-module-dev]: module-development.md
