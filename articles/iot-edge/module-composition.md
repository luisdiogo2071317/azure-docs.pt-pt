---
title: Composição do módulo do IoT Edge do Azure | Documentos da Microsoft
description: Saiba como um manifesto de implantação declara que módulos para implementar, como implementá-los e como criar rotas de mensagens entre eles.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 12c53b1fdad4ab8f55c000ca1cb4f08dab7c8a74
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51230378"
---
# <a name="learn-how-to-deploy-modules-and-establish-routes-in-iot-edge"></a>Saiba como implementar módulos e estabelecer as rotas no IoT Edge

Cada dispositivo IoT Edge é executado, pelo menos, dois módulos: $edgeAgent e $edgeHub, que compõem o runtime do IoT Edge. Além disso, qualquer dispositivo IoT Edge pode executar vários módulos para executar qualquer número de processos. Quando implementa todos esses módulos para um dispositivo ao mesmo tempo, precisa de uma forma para declarar os módulos a serem incluídos e como eles interagem entre si. 

O *manifesto de implantação* é um documento JSON que descreve:

* A configuração do agente do Edge, que inclui a imagem de contentor para cada módulo, as credenciais para registos de contentores privado de acesso e instruções sobre como cada módulo deve ser criado e gerido.
* A configuração do hub do Edge, que inclui a forma como o fluxo de mensagens entre módulos e, eventualmente, para o IoT Hub.
* Opcionalmente, as propriedades pretendidas do duplos de módulo.

Todos os dispositivos do IoT Edge tem de ser configurado com um manifesto de implantação. Os relatórios de um tempo de execução do IoT Edge instalado recentemente um código de erro até que configurado com um manifesto válido. 

Os tutoriais do Azure IoT Edge, vai criar um manifesto de implantação através de um assistente no portal do Azure IoT Edge. Também pode aplicar um manifesto de implantação por meio de programação através do REST ou o SDK do serviço Hub IoT. Para obter mais informações, consulte [implementações do IoT Edge compreender](module-deployment-monitoring.md).

## <a name="create-a-deployment-manifest"></a>Criar um manifesto de implantação

Num alto nível, o manifesto de implantação configura as propriedades pretendidas do duplo do módulo de módulos do IoT Edge implementados num dispositivo IoT Edge. Dois destes módulos são sempre presente: `$edgeAgent`, e `$edgeHub`.

Um manifesto de implantação que contém apenas o runtime do IoT Edge (agente e hub) é válido.

O manifesto segue essa estrutura:

```json
{
    "modulesContent": {
        "$edgeAgent": {
            "properties.desired": {
                // desired properties of the Edge agent
                // includes the image URIs of all modules
                // includes container registry credentials
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

Precisa informar ao runtime do IoT Edge como instalar os módulos na sua implementação. As informações de configuração e gestão de todos os módulos penetra no **$edgeAgent** propriedades pretendidas. Estas informações incluem os parâmetros de configuração para o agente do Edge em si. 

Para obter uma lista completa de propriedades que pode ou tem de ser incluída, consulte [propriedades do agente do Edge e hub do Edge](module-edgeagent-edgehub.md).

As propriedades de $edgeAgent siga esta estrutura:

```json
"$edgeAgent": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
            "settings":{
                "registryCredentials":{ // give the edge agent access to container images that aren't public
                    }
                }
            }
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
            "{module1}": { // optional
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

Hub do Edge fornece uma forma de forma declarativa a rotear mensagens entre módulos e entre módulos e o IoT Hub. O hub do Edge gerencia todas as comunicações, para que as informações de rota penetra no **$edgeHub** propriedades pretendidas. Pode ter várias rotas dentro da mesma implementação.

As rotas são declaradas na **$edgeHub** pretendido propriedades com a seguinte sintaxe:

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

Cada rota tem uma origem e sink, mas a condição é uma parte opcional que pode utilizar para filtrar as mensagens. 


### <a name="source"></a>Origem
A origem Especifica de onde vêm as mensagens. Pode ser qualquer um dos seguintes valores:

| Origem | Descrição |
| ------ | ----------- |
| `/*` | Todas as mensagens de dispositivo para a cloud de qualquer dispositivo ou um módulo |
| `/messages/*` | Qualquer mensagem de dispositivo-para-cloud enviada por um dispositivo ou um módulo por meio de alguns ou nenhum resultado |
| `/messages/modules/*` | Qualquer mensagem de dispositivo-para-cloud enviada por um módulo por meio de alguns ou nenhum resultado |
| `/messages/modules/{moduleId}/*` | Qualquer mensagem de dispositivo para a cloud enviada pelo {moduleId} sem saída |
| `/messages/modules/{moduleId}/outputs/*` | Qualquer mensagem de dispositivo para a cloud enviada por {moduleId} com algumas saídas |
| `/messages/modules/{moduleId}/outputs/{output}` | Qualquer mensagem de dispositivo para a cloud enviada através da utilização de {moduleId} {saída} |

### <a name="condition"></a>Condição
A condição é opcional numa declaração de rota. Se quiser passar todas as mensagens de coletor para a origem, basta omitir a **onde** cláusula inteiramente. Ou pode utilizar o [linguagem de consulta do IoT Hub](../iot-hub/iot-hub-devguide-routing-query-syntax.md) para filtrar determinadas mensagens ou tipos de mensagem que satisfaçam a condição.

As mensagens que passam entre módulos no IoT Edge são formatadas as mesmas que as mensagens que passam entre os dispositivos e IoT Hub do Azure. Todas as mensagens são formatadas como JSON e ter **systemProperties**, **appProperties**, e **corpo** parâmetros. 

Pode criar consultas em torno de todos os três parâmetros com a seguinte sintaxe: 

* Propriedades do sistema: `$<propertyName>` ou `{$<propertyName>}`
* Propriedades da aplicação: `<propertyName>`
* Propriedades do corpo: `$body.<propertyName>` 

Para obter exemplos sobre como criar consultas para as propriedades da mensagem, consulte [rotas de expressões de consulta de mensagem de dispositivo-para-cloud](../iot-hub/iot-hub-devguide-routing-query-syntax.md).

Um exemplo que é específico do IoT Edge é quando se deseja para filtrar as mensagens que chegaram a um dispositivo de gateway, a partir de um dispositivo de folha. As mensagens provenientes de módulos contêm uma propriedade de sistema chamada **connectionModuleId**. Então, se pretender encaminhar mensagens a partir de dispositivos de folha diretamente para o IoT Hub, utilize a rota seguinte para excluir as mensagens do módulo:

```sql
FROM /messages/\* WHERE NOT IS_DEFINED($connectionModuleId) INTO $upstream
```

### <a name="sink"></a>Sink
O sink define onde as mensagens são enviadas. Pode ser qualquer um dos seguintes valores:

| Sink | Descrição |
| ---- | ----------- |
| `$upstream` | Enviar a mensagem para o IoT Hub |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Enviar a mensagem de entrada `{input}` do módulo `{moduleId}` |

IoT Edge fornece garantias pelo-menos-uma vez. O hub do Edge armazena mensagens localmente no caso de uma rota não é possível entregar a mensagem para o sink. Por exemplo, se o hub do Edge não é possível ligar ao IoT Hub ou o módulo de destino não está ligado.

Hub do Edge armazena as mensagens até à hora especificada no `storeAndForwardConfiguration.timeToLiveSecs` propriedade o [propriedades pretendidas do hub do Edge](module-edgeagent-edgehub.md).

## <a name="define-or-update-desired-properties"></a>Definir ou atualizar as propriedades pretendidas 

O manifesto de implantação pode especificar as propriedades pretendidas para o módulo duplo de cada módulo implementado no dispositivo IoT Edge. Quando as propriedades pretendidas são especificadas no manifesto de implantação, eles substituem quaisquer propriedades pretendidas atualmente no duplo do módulo.

Se não especificar as propriedades de pretendidas do duplo do módulo no manifesto de implantação, IoT Hub não irá modificar o duplo do módulo de qualquer forma, e poderá definir as propriedades pretendidas por meio de programação.

Os mesmos mecanismos que permitem modificar dispositivos duplos são utilizados para modificar duplos de módulo. Para obter mais informações, consulte a [Guia do programador do módulo duplo](../iot-hub/iot-hub-devguide-module-twins.md).   

## <a name="deployment-manifest-example"></a>Exemplo de manifesto de implantação

Este exemplo de um documento JSON de manifesto de implantação.

```json
{
  "modulesContent": {
    "$edgeAgent": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
          "type": "docker",
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "ContosoRegistry": {
                "username": "myacr",
                "password": "{password}",
                "address": "myacr.azurecr.io"
              }
            }
          }
        },
        "systemModules": {
          "edgeAgent": {
            "type": "docker",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
              "createOptions": ""
            }
          },
          "edgeHub": {
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
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
              "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
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

* Para obter uma lista completa das propriedades que podem ou têm de ser incluídos na $edgeAgent e $edgeHub, consulte [propriedades do agente do Edge e hub do Edge](module-edgeagent-edgehub.md).

* Agora que sabe como os módulos do IoT Edge são usados, [compreender os requisitos e as ferramentas para desenvolver módulos do IoT Edge](module-development.md).
