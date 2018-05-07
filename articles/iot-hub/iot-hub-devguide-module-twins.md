---
title: Compreender duplos de módulo do Azure IoT Hub | Microsoft Docs
description: Guia para programadores - duplos do módulo de utilização para sincronizar dados de estado e a configuração entre o IoT Hub e dos dispositivos
services: iot-hub
documentationcenter: .net
author: chrissie926
manager: timlt
editor: ''
ms.assetid: 8a3da072-a5bf-46e5-8de4-24cdbb2a03fa
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: menchi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6fbbce06653e60cd914c2ed4d5990aac78ef53a8
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="understand-and-use-module-twins-in-iot-hub"></a>Compreender e utilizar duplos módulo no IoT Hub

Este artigo pressupõe que já leu [compreender e utilizar dispositivos duplos no IoT Hub] [ lnk-devguide-device-twins] primeiro. No IoT Hub, em cada identidade de dispositivo, pode criar até 20 identidades do módulo. Cada identidade de módulo gera implicitamente um duplo de módulo. Muito semelhantes para dispositivos duplos, duplos módulo são documentos JSON que armazenam informações de estado do módulo, incluindo metadados, configurações e condições. IoT Hub do Azure mantém um duplo de módulo para cada módulo que se liga ao IoT Hub. 

No lado do dispositivo, os SDKs do dispositivo IoT Hub permitem-lhe criar os módulos que cada abre uma ligação independente ao IoT Hub. Isto permite-lhe utilizar espaços de nomes separados de diferentes componentes no seu dispositivo. Por exemplo, tem uma máquina de distribuidores automáticos que tenha três sensores diferentes. Cada sensor é controlada pelas diferentes departamentos da sua empresa. Pode criar um módulo para cada sensor. Desta forma, cada departamento é apenas capaz de enviar tarefas ou métodos diretos para o sensor controlarem, evitando entra em conflito e erros de utilizador.

 Duplo de identidade e o módulo módulo fornece as mesmas capacidades que o dispositivo duplo de identidade e devic mas granularidade melhorar. Este granularidade melhorar permite compatível com dispositivos, tais como o sistema operativo com base em dispositivos ou os dispositivos de firmware gerir vários componentes, a isolar a configuração e as condições para cada um desses componentes. Identidade do módulo e duplos módulo proporcionam uma separação de gestão das preocupações ao trabalhar com dispositivos de IoT que tenham componentes de software modulares. Pretendemos em todas as funcionalidades dispositivo duplo ao nível de duplo do módulo de suporte através da disponibilidade geral do módulo duplo. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Este artigo descreve:

* A estrutura do duplo de módulo: *etiquetas*, *pretendido* e *comunicadas propriedades*.
* As operações de módulos e back-ends podem executar nos duplos do módulo.

Consulte [orientações de comunicação do dispositivo para nuvem] [ lnk-d2c-guidance] para obter orientações sobre como utilizar propriedades comunicadas, mensagens do dispositivo para nuvem ou carregamento do ficheiro.
Consulte [orientações de comunicação de nuvem para o dispositivo] [ lnk-c2d-guidance] para obter orientações sobre como utilizar propriedades pretendidas, métodos diretos ou mensagens da nuvem para o dispositivo.

## <a name="module-twins"></a>Duplos do módulo
Duplos módulo armazenam informações relacionadas com o módulo que:

* Módulos no dispositivo e do IoT Hub podem utilizar para sincronizar as condições de módulo e a configuração.
* O solução de back-end pode utilizar para consulta e de destino demoradas operações.

O ciclo de vida de um duplo módulo ligado correspondente [identidade do módulo][lnk-identity]. Duplos módulos implicitamente são criados e eliminados quando uma identidade do módulo é criada ou eliminada no IoT Hub.

Um duplo do módulo é um documento JSON que inclui:

* **Etiquetas**. Uma secção do documento JSON que a solução de back-end pode leem e escrevem para. As etiquetas não são visíveis para os módulos no dispositivo. As etiquetas são definidas para consultar o objetivo.
* **Pretender propriedades**. Utilizado juntamente com propriedades comunicadas para sincronizar a configuração do módulo ou condições. O solução de back-end pode definir propriedades pretendidas e, a aplicação de módulo pode lê-los. A aplicação de módulo também pode receber notificações de alterações nas propriedades de pretendido.
* **Comunicado propriedades**. Utilizado juntamente com propriedades pretendidas para sincronizar a configuração do módulo ou condições. A aplicação de módulo pode definir propriedades comunicadas e o solução de back-end pode ler e consultá-los.
* **Propriedades de identidade do módulo**. A raiz do documento módulo duplo JSON contém as propriedades só de leitura de identidade do módulo correspondente armazenado no [registo de identidade][lnk-identity].

![][img-module-twin]

O exemplo seguinte mostra um duplo módulo documento JSON:

```json
{
    "deviceId": "devA",
    "moduleId": "moduleA",
    "etag": "AAAAAAAAAAc=", 
    "status": "enabled",
    "statusReason": "provisioned",
    "statusUpdateTime": "0001-01-01T00:00:00",
    "connectionState": "connected",
    "lastActivityTime": "2015-02-30T16:24:48.789Z",
    "cloudToDeviceMessageCount": 0, 
    "authenticationType": "sas",
    "x509Thumbprint": {     
        "primaryThumbprint": null, 
        "secondaryThumbprint": null 
    }, 
    "version": 2, 
    "tags": {
        "$etag": "123",
        "deploymentLocation": {
            "building": "43",
            "floor": "1"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            "$metadata" : {...},
            "$version": 1
        },
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            "batteryLevel": 55,
            "$metadata" : {...},
            "$version": 4
        }
    }
}
```

O objeto de raiz é o módulo de propriedades de identidade e contentor de objetos para `tags` e ambos `reported` e `desired` propriedades. O `properties` contentor contém alguns elementos de só de leitura (`$metadata`, `$etag`, e `$version`) descrito a [dos metadados do módulo duplo] [ lnk-module-twin-metadata] e [ Simultaneidade otimista] [ lnk-concurrency] secções.

### <a name="reported-property-example"></a>Exemplo de propriedade comunicado
No exemplo anterior, o duplo do módulo contém um `batteryLevel` propriedade que é comunicada pela aplicação do módulo. Esta propriedade torna possível consultar e operar em módulos com base no nível de bateria comunicado último. Outros exemplos incluem as capacidades de módulo de relatórios de aplicações do módulo ou as opções de conectividade.

> [!NOTE]
> Propriedades comunicadas simplificam cenários em que a solução de back-end é interessado no último valor conhecido de uma propriedade. Utilize [mensagens do dispositivo para nuvem] [ lnk-d2c] se a solução de back-end necessita de processar a telemetria de módulo sob a forma de sequências de eventos de timestamped, tais como a série de tempo.

### <a name="desired-property-example"></a>Exemplo de propriedade pretendida
No exemplo anterior, o `telemetryConfig` duplo módulo pretendida e propriedades comunicadas são utilizadas pela solução de back-end e a aplicação do módulo para sincronizar a configuração de telemetria para este módulo. Por exemplo:

1. O solução de back-end define a propriedade com o valor de configuração pretendida pretendida. Segue-se a parte do documento com a propriedade pretendido definida:

    ```json
    ...
    "desired": {
        "telemetryConfig": {
            "sendFrequency": "5m"
        },
        ...
    },
    ...
    ```

2. A aplicação do módulo é notificada da alteração imediatamente se ligado ou, no primeiro o restabelecimento de ligação. A aplicação de módulo, em seguida, reporta a configuração atualizada (ou uma condição de erro utilizando o `status` propriedade). Segue-se a parte das propriedades comunicadas:

    ```json
    ...
    "reported": {
        "telemetryConfig": {
            "sendFrequency": "5m",
            "status": "success"
        }
        ...
    }
    ...
    ```

3. O solução de back-end pode controlar os resultados da operação de configuração em muitos módulos, por [consultar] [ lnk-query] duplos do módulo.

> [!NOTE]
> Os fragmentos anteriores são exemplos, otimizados para legibilidade, de uma forma de codificar uma configuração do módulo e o respetivo estado. IoT Hub não impõe um esquema específico para o duplo módulo pretendida e reportado propriedades no duplos módulo.
> 
> 

## <a name="back-end-operations"></a>Operações de back-end
O solução de back-end funciona com o duplo módulo utilizando as seguintes operações atómicas, expostas através de HTTPS:

* **Obter duplo módulo por ID**. Esta operação devolve o documento de duplo módulo, incluindo etiquetas e propriedades do sistema pretendido e comunicados.
* **Atualizar parcialmente duplo módulo**. Esta operação permite que a solução de back-end parcialmente atualizar as etiquetas ou propriedades pretendidas no duplo módulo. A atualização parcial é expresso como um documento JSON que adiciona ou atualiza uma propriedade. As propriedades definidas como `null` são removidos. O exemplo seguinte cria uma nova propriedade pretendida com valor `{"newProperty": "newValue"}`, substitui o valor existente da `existingProperty` com `"otherNewValue"`e remove `otherOldProperty`. Não existem outras alterações são efetuadas às propriedades pretendidas existentes ou tags:

    ```json
    {
        "properties": {
            "desired": {
                "newProperty": {
                    "nestedProperty": "newValue"
                },
                "existingProperty": "otherNewValue",
                "otherOldProperty": null
            }
        }
    }
    ```

* **Substituir propriedades pretendidas**. Esta operação permite que a solução de back-end para completamente substituir propriedades pretendidas todas as existentes e substituir um novo documento JSON para `properties/desired`.
* **Substitua as etiquetas**. Esta operação permite que a solução de back-end para completamente substituir a existentes todas as etiquetas e a substituir um novo documento JSON para `tags`.
* **Receber notificações de duplo**. Esta operação permite que a solução de back-end ser notificado quando o duplo é modificado. Para tal, a solução de IoT tem de criar uma rota e definir a origem de dados igual a *twinChangeEvents*. Por predefinição, não existem notificações duplo são enviadas, ou seja, sem estas rotas existem previamente. Se a taxa de alteração é demasiado elevada, ou por outros motivos como falhas internos, o IoT Hub poderá enviar apenas uma notificação que contém todas as alterações. Por conseguinte, se a sua aplicação tiver fiável de auditoria e registo de todos os Estados intermédios, deve utilizar mensagens do dispositivo para nuvem. A mensagem de notificação de duplo inclui as propriedades e corpo.

    - Propriedades

    | Nome | Valor |
    | --- | --- |
    $content-tipo | application/json |
    $iothub-enqueuedtime |  Hora em que a notificação foi enviada |
    $iothub-mensagem-origem | twinChangeEvents |
    $content-encoding | UTF-8 |
    deviceId | ID do dispositivo |
    moduleId | ID do módulo |
    hubName | Nome do IoT Hub |
    operationTimestamp | [ISO8601] timestamp da operação |
    iothub-message-schema | deviceLifecycleNotification |
    opType | "replaceTwin" ou "updateTwin" |

    Propriedades do sistema de mensagens têm o prefixo de `'$'` símbolo.

    - Corpo
        
    Esta secção inclui todas as alterações de duplo num formato JSON. Utiliza o mesmo formato como uma correção, com a diferença que pode conter todas as secções de duplo: etiquetas, properties.reported, properties.desired e que contém os elementos de "$metadata". Por exemplo,

    ```json
    {
        "properties": {
            "desired": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            },
            "reported": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            }
        }
    }
    ```

Todas as operações anteriores suportam [simultaneidade otimista] [ lnk-concurrency] e requerem o **ServiceConnect** permissão, tal como definido no [segurança] [ lnk-security] artigo.

Além destas operações, o solução de back-end pode:

* Consultar os duplos de módulo com o tipo SQL, [idioma de consulta do IoT Hub][lnk-query].

## <a name="module-operations"></a>Operações de módulos
A aplicação de módulo funciona com o duplo módulo utilizando as seguintes operações atómicas:

* **Obter duplo módulo**. Esta operação devolve o documento de duplo do módulo (incluindo etiquetas e propriedades do sistema pretendido e comunicados) para o módulo atualmente ligado.
* **Parcialmente atualizar propriedades comunicadas**. Esta operação permite que a atualização parcial das propriedades comunicadas do módulo do atualmente ligado. Esta operação utiliza o mesmo formato de atualização JSON que a solução de fazer uma cópia de utilizações de fim de uma atualização parcial das propriedades pretendidas.
* **Observar propriedades pretendidas**. O módulo atualmente ligado pode optar por ser notificado das atualizações para as propriedades pretendidas quando que ocorrem. O módulo recebe o mesmo formato de atualização (substituição parcial ou completa) executado pela solução de back-end.

Todas as operações anteriores requerem o **ModuleConnect** permissão, tal como definido no [segurança] [ lnk-security] artigo.

O [SDKs do Azure IoT device] [ lnk-sdks] torna mais fácil de utilizar as operações anteriores a partir de vários idiomas e plataformas.

## <a name="tags-and-properties-format"></a>Formato de etiquetas e propriedades
As etiquetas, propriedades pretendidas e propriedades comunicadas são objetos JSON com as seguintes restrições:

* Todas as chaves em objetos JSON são sensíveis a maiúsculas 64 bytes cadeias UNICODE UTF-8. Permitido carateres excluir carateres de controlo UNICODE (segmentos C0 e C1), e `'.'`, `' '`, e `'$'`.
* Todos os valores em objetos JSON podem ser dos seguintes tipos de JSON: boolean, número, cadeia, objecto. Não são permitidas matrizes. O valor máximo de números inteiros é 4503599627370495 e o valor mínimo de números inteiros é-4503599627370496.
* Todos os objetos JSON etiquetas, propriedades pretendidas e reportadas podem ter uma profundidade máxima de 5. Por exemplo, do seguinte objeto é válido:

    ```json
    {
        ...
        "tags": {
            "one": {
                "two": {
                    "three": {
                        "four": {
                            "five": {
                                "property": "value"
                            }
                        }
                    }
                }
            }
        },
        ...
    }
    ```

* Todos os valores de cadeia podem ser, no máximo 4 KB de comprimento.

## <a name="module-twin-size"></a>Tamanho do módulo duplo
IoT Hub impõe uma limitação de tamanho de 8KB em cada um dos respetivos valores totais de `tags`, `properties/desired`, e `properties/reported`, excluindo os elementos de só de leitura.
O tamanho é calculado através da contagem de todos os carateres, excluindo carateres de controlo UNICODE (segmentos C0 e C1) e espaços que estejam fora as constantes string.
IoT Hub com um erro rejeita todas as operações que podem aumentar o tamanho desses documentos supera o limite.

## <a name="module-twin-metadata"></a>Metadados do módulo duplo
IoT Hub mantém o carimbo da última atualização para cada objeto JSON no duplo módulo pretendida e reportado propriedades. Os carimbos está em UTC e codificadas no [ISO8601] formato `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Por exemplo:

```json
{
    ...
    "properties": {
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            "$metadata": {
                "telemetryConfig": {
                    "sendFrequency": {
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$lastUpdated": "2016-03-30T16:24:48.789Z"
                },
                "$lastUpdated": "2016-03-30T16:24:48.789Z"
            },
            "$version": 23
        },
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            "batteryLevel": "55%",
            "$metadata": {
                "telemetryConfig": {
                    "sendFrequency": "5m",
                    "status": {
                        "$lastUpdated": "2016-03-31T16:35:48.789Z"
                    },
                    "$lastUpdated": "2016-03-31T16:35:48.789Z"
                }
                "batteryLevel": {
                    "$lastUpdated": "2016-04-01T16:35:48.789Z"
                },
                "$lastUpdated": "2016-04-01T16:24:48.789Z"
            },
            "$version": 123
        }
    }
    ...
}
```

Esta informação é mantida em cada nível (não apenas os leaves da estrutura de JSON) para manter as atualizações que remover chaves de objeto.

## <a name="optimistic-concurrency"></a>Simultaneidade otimista
Etiquetas, assim o desejar e comunicadas propriedades todos os simultaneidade otimista de suporte.
Etiquetas tem uma ETag, como por [RFC7232], que representa a representação JSON da etiqueta. Pode utilizar ETags em operações de actualização condicional da solução de back-end para garantir consistência.

Duplo módulo pretendida e reportado a propriedades não dispõe de ETags, mas tem um `$version` valor que fique incremental. Da mesma forma para uma ETag, a versão pode ser utilizada por terceiros a atualização para impor a consistência das atualizações. Por exemplo, uma aplicação do módulo para uma propriedade que relatados ou o solução de back-end para uma propriedade pretendido.

Versões também são úteis quando um agente observing (por exemplo, a aplicação de módulo observar as propriedades pretendidas) tem reconciliar races entre o resultado de uma operação de obtenção e uma notificação de atualização. A secção [fluxo de restabelecimento de ligação do dispositivo] [lnk-restabelecimento de ligação] fornece mais informações. 

## <a name="next-steps"></a>Passos Seguintes
Para experimentar alguns dos conceitos descritos neste artigo, consulte os seguintes tutoriais do IoT Hub:

* [Introdução ao IoT Hub módulo identidade e o módulo duplo utilizando a cópia de segurança do .NET e o dispositivo de .NET][lnk-module-twin-tutorial]

<!-- links and images -->

[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232

[lnk-module-twin-tutorial]: iot-hub-csharp-csharp-module-twin-getstarted.md
[lnk-module-twin-metadata]: iot-hub-devguide-module-twins.md#module-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[img-module-twin]: media/iot-hub-devguide-device-twins/module-twin.jpg
