---
title: Compreender os métodos diretos do IoT Hub do Azure | Documentos da Microsoft
description: Guia do programador - utilizar métodos diretos para invocar código nos seus dispositivos a partir de uma aplicação de serviço.
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: nberdy
ms.openlocfilehash: 881262816fc8bd634b7f577fd05aa0c8c062e4ca
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126529"
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Compreender e invocar métodos diretos do IoT Hub
IoT Hub dá-lhe a capacidade de invocar métodos diretos em dispositivos a partir da cloud. Os métodos diretos representam uma interação de solicitação-resposta com um dispositivo semelhante a uma chamada HTTP em que forem bem-sucedidos ou falharem imediatamente (após um tempo limite especificado pelo utilizador). Essa abordagem é útil para cenários em que o método de ação imediata é diferente, dependendo se o dispositivo foi capaz de responder.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Cada método de dispositivo destina-se um único dispositivo. [Trabalhos] [ lnk-devguide-jobs] proporcionam uma forma de invocar métodos diretos em vários dispositivos e agendar a invocação de método para os dispositivos desligados.

Qualquer pessoa com **ligação de serviço** permissões no IoT Hub podem invocar um método num dispositivo.

Os métodos diretos seguem um padrão de solicitação-resposta e destinam-se para as comunicações que necessitam de confirmação imediata do seu resultado. Por exemplo, interativo controlo do dispositivo, como ativar um fã.

Consulte a [documentação de orientação do Cloud-para-dispositivo comunicação] [ lnk-c2d-guidance] em caso de dúvida, entre com as propriedades pretendidas, direcionar métodos ou mensagens na cloud para o dispositivo.

## <a name="method-lifecycle"></a>Ciclo de vida do método
Os métodos diretos são implementados no dispositivo e podem exigir a zero ou mais entradas no payload de método para instanciar corretamente. Invoca um método direto através de um URI de serviço com acesso à (`{iot hub}/twins/{device id}/methods/`). Um dispositivo recebe métodos diretos através de um tópico MQTT específicos do dispositivo (`$iothub/methods/POST/{method name}/`) ou através de ligações AMQP (`IoThub-methodname` e `IoThub-status` propriedades da aplicação). 

> [!NOTE]
> Quando invoca um método direto num dispositivo, os nomes das propriedades e valores podem apenas conter US-ASCII imprimível alfanumérico, exceto as no conjunto de seguinte: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

Direcionar os métodos são síncronos e o êxito ou falhar após o período de tempo limite (predefinição: 30 segundos, definíveis cópia de segurança para 3600 segundos). Os métodos diretos são úteis em cenários interativos onde pretende que um dispositivo para agir se e apenas se o dispositivo está online e de recebimento de comandos. Por exemplo, ativando uma luz de um telefone. Nestes cenários, pretende ver um imediato êxito ou falha, para que o serviço em nuvem pode agir sobre o resultado assim que possível. O dispositivo pode retornar alguns corpo da mensagem como resultado do método, mas não é necessário para o método fazer isso. Não existe nenhuma garantia na ordem ou qualquer semântica de simultaneidade em chamadas de método.

Direcionar os métodos são apenas de HTTPS do lado da nuvem e MQTT ou AMQP do lado do dispositivo.

O payload de método solicitações e respostas é um JSON até 128 KB de documentos.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Invocar um método direto a partir de uma aplicação de back-end
### <a name="method-invocation"></a>Invocação de método
Invocações de método direto num dispositivo são chamadas HTTPS, que incluem:

* O *URI do pedido* específico para o dispositivo juntamente com o [versão de API](/rest/api/iothub/service/invokedevicemethod):

    ```http
    https://fully-qualified-iothubname.azure-devices.net/twins/{deviceId}/methods?api-version=2018-06-30
    ```

* A POSTAGEM *método*
* *Cabeçalhos* que contém a autorização, ID de tipo de conteúdo e codificação de conteúdo do pedido
* Um JSON transparente *corpo* no seguinte formato:

    ```json
    {
        "methodName": "reboot",
        "responseTimeoutInSeconds": 200,
        "payload": {
            "input1": "someInput",
            "input2": "anotherInput"
        }
    }
    ```

Tempo limite é em segundos. Se o tempo limite não for definida, é assumida como predefinição para 30 segundos.

#### <a name="example"></a>Exemplo

Veja a seguir um exemplo de barebone usando `curl`. 

```bash
curl -X POST \
  https://iothubname.azure-devices.net/twins/myfirstdevice/methods?api-version=2018-06-30 \
  -H 'Authorization: SharedAccessSignature sr=iothubname.azure-devices.net&sig=x&se=x&skn=iothubowner' \
  -H 'Content-Type: application/json' \
  -d '{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}'
```

### <a name="response"></a>Resposta
A aplicação de back-end recebe uma resposta que abrange:

* *Código de estado HTTP*, que é utilizado para erros proveniente do IoT Hub, incluindo um erro 404 para dispositivos não estão atualmente ligadas
* *Cabeçalhos* que contém a ETag, ID de tipo de conteúdo e codificação de conteúdo do pedido
* Um JSON *corpo* no seguinte formato:

    ```json
    {
        "status" : 201,
        "payload" : {...}
    }
    ```

    Ambos `status` e `body` são fornecidos pelo dispositivo e utilizados para responder com o código de estado do dispositivo e/ou a descrição.

### <a name="method-invocation-for-iot-edge-modules"></a>Invocação de método para módulos do IoT Edge
A invocar métodos diretos, utilizar um módulo de ID é suportado na linguagem c# SDK de pré-visualização (disponíveis [aqui](https://www.nuget.org/packages/Microsoft.Azure.Devices/1.16.0-preview-004)).

Para este efeito, utilize o `ServiceClient.InvokeDeviceMethodAsync()` método e passe a `deviceId` e `moduleId` como parâmetros.

## <a name="handle-a-direct-method-on-a-device"></a>Lidar com um método direto num dispositivo
### <a name="mqtt"></a>MQTT
#### <a name="method-invocation"></a>Invocação de método
Os dispositivos recebem pedidos de método direto sobre o tópico MQTT: `$iothub/methods/POST/{method name}/?$rid={request id}`

O corpo que recebe o dispositivo é o seguinte formato:

```json
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Pedidos de método são QoS 0.

#### <a name="response"></a>Resposta
O dispositivo envia as respostas a `$iothub/methods/res/{status}/?$rid={request id}`, em que:

* O `status` propriedade é o estado fornecido pelo dispositivo de execução do método.
* O `$rid` propriedade é o ID do pedido de invocação de método recebido do IoT Hub.

O corpo é definido pelo dispositivo e pode ser qualquer Estado.

### <a name="amqp"></a>AMQP
#### <a name="method-invocation"></a>Invocação de método
O dispositivo recebe pedidos de método direto ao criar uma ligação de recebimento no endereço `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

A mensagem AMQP chega-se no link de recebimento que representa o pedido de método. Ele contém o seguinte:
* A propriedade de ID de correlação, que contém um ID de pedido que deve ser passado de volta com a resposta do método correspondente
* Uma propriedade de aplicativo chamada `IoThub-methodname`, que contém o nome do método sendo invocado
* O corpo da mensagem AMQP que contêm a carga do método como JSON

#### <a name="response"></a>Resposta
O dispositivo cria uma ligação de envio para retornar a resposta do método no endereço `amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

Resposta do método é retornada no link de envio e está estruturada da seguinte forma:
* A propriedade de ID de correlação, que contém o ID do pedido é transmitido na mensagem de pedido do método
* Uma propriedade de aplicativo chamada `IoThub-status`, que contém o utilizador fornecido pelo Estado do método
* O corpo da mensagem AMQP que contém a resposta do método como JSON

## <a name="additional-reference-material"></a>Material de referência adicionais
Outros tópicos de referência no Guia do programador do IoT Hub incluem:

* [Pontos finais do IoT Hub] [ lnk-endpoints] descreve vários pontos de extremidade que cada hub IoT expõe para operações de tempo de execução e gestão.
* [Quotas e limitação] [ lnk-quotas] descreve as quotas que se aplicam e o comportamento da limitação esperar ao utilizar o IoT Hub.
* [Azure SDKs de dispositivo e de serviços de IoT] [ lnk-sdks] indica o idioma de vários SDKs, pode utilizar ao desenvolver aplicações de dispositivos e de serviços que interagem com o IoT Hub.
* [Linguagem de consulta do IoT Hub para dispositivos duplos, tarefas e encaminhamento de mensagens] [ lnk-query] descreve a linguagem de consulta do IoT Hub pode utilizar para recuperar informações a partir do IoT Hub sobre os seus dispositivos duplos e trabalhos.
* [Suporte para MQTT do IoT Hub] [ lnk-devguide-mqtt] fornece mais informações sobre o suporte do IoT Hub para o protocolo MQTT.

## <a name="next-steps"></a>Passos Seguintes
Agora, aprendeu a utilizar métodos diretos, poderá estar interessado no seguinte artigo do guia de programador do IoT Hub:

* [Programar tarefas em vários dispositivos][lnk-devguide-jobs]

Se quiser experimentar alguns dos conceitos descritos neste artigo, poderá estar interessado no tutorial seguinte do IoT Hub:

* [Utilizar métodos diretos][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
