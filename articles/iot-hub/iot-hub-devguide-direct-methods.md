---
title: "Compreender os métodos diretos do IoT Hub do Azure | Microsoft Docs"
description: "Guia para programadores - utilize métodos direta invocar código nos seus dispositivos a partir de uma aplicação de serviço."
services: iot-hub
documentationcenter: .net
author: nberdy
manager: timlt
editor: 
ms.assetid: 9f0535f1-02e6-467a-9fc4-c0950702102d
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/19/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 243845139c7ae0389333d7490098ef73f95dceac
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a>Compreender e invocar métodos diretos do IoT Hub
IoT Hub permite invocar métodos diretos nos dispositivos da nuvem. Métodos diretos representam uma interação de pedido-resposta com um dispositivo semelhante a uma chamada HTTP em que são ou não bem-sucedidos imediatamente (após um tempo limite especificado de um utilizador). Esta abordagem é útil para cenários em que o método de ação imediata é diferente consoante se o dispositivo foi capaz de responder, como o envio de uma reativação SMS para um dispositivo se um dispositivo estiver offline (que está a ser mais dispendioso do que uma chamada de método SMS).
Cada método de dispositivo está direcionada para um único dispositivo. [As tarefas] [ lnk-devguide-jobs] proporcionam uma forma de invocar métodos diretos em vários dispositivos e agendar a invocação de métodos para dispositivos desligados.

Qualquer pessoa com **serviço ligar** permissões no IoT Hub podem invocar um método num dispositivo.

Os métodos diretos seguem um padrão de resposta-pedido e destinam-se para as comunicações que necessitam de confirmação imediata do respetivo resultado, o controlo normalmente interativo do dispositivo, por exemplo, para ativar uma ventoinha.

Consulte [orientações de comunicação de nuvem para o dispositivo] [ lnk-c2d-guidance] em caso de dúvida entre a utilização pretendidas propriedades, direcionar a mensagens da nuvem para o dispositivo ou métodos.

## <a name="method-lifecycle"></a>Ciclo de vida do método
Métodos diretos são implementados no dispositivo e podem necessitar de zero ou mais entradas no payload do método ao instanciar corretamente. Invocar um método direto através de um URI de serviço com acesso à (`{iot hub}/twins/{device id}/methods/`). Um dispositivo recebe métodos diretos através de um tópico MQTT específicos do dispositivo (`$iothub/methods/POST/{method name}/`) ou através de ligações AMQP (`IoThub-methodname` e `IoThub-status` propriedades da aplicação). 

> [!NOTE]
> Quando invocar um método direto num dispositivo, valores e nomes de propriedade podem apenas conter imprimível US-ASCII alfanuméricos, exceto qualquer no conjunto de seguinte: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

Direcionar métodos são síncronos e o êxito ou falhar depois do período de tempo limite (predefinição: 30 segundos, pode ser definidos cópias de segurança para 3600 segundos). Métodos diretos são úteis em cenários interativos onde pretende que um dispositivo para atuar se e apenas se o dispositivo está online e receber comandos, como ativar uma leve do telefone. Nestes cenários, pretender ver um imediato êxito ou falha, pelo que o serviço em nuvem pode atuar no resultado logo que possível. O dispositivo poderá devolver algumas corpo da mensagem em resultado de método, mas não é necessária para o método para o fazer. Não há nenhuma garantia na ordenação ou qualquer semântica de concorrência em chamadas de método.

Direcionar métodos são HTTPS apenas do lado da nuvem e MQTT ou AMQP do lado do dispositivo.

O payload de pedidos de método e as respostas é um JSON documento até 128 KB.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Invocar um método direto a partir de uma aplicação de back-end
### <a name="method-invocation"></a>Invocação de métodos
Invocações de método direto num dispositivo são chamadas HTTPS, que incluem:

* O *URI* específico para o dispositivo (`{iot hub}/twins/{device id}/methods/`)
* A publicação *método*
* *Cabeçalhos* que contém a autorização, pedir ID, tipo de conteúdo e codificação de conteúdo
* Um JSON transparente *corpo* no seguinte formato:

   ```
   {
       "methodName": "reboot",
       "responseTimeoutInSeconds": 200,
       "payload": {
           "input1": "someInput",
           "input2": "anotherInput"
       }
   }
   ```

Tempo limite é em segundos. Se o tempo limite não for definido, assume como 30 segundos.

### <a name="response"></a>Resposta
A aplicação de back-end recebe uma resposta que inclui:

* *Código de estado HTTP*, que é utilizado para erros proveniente do IoT Hub, incluindo um erro 404 para dispositivos atualmente não ligado
* *Cabeçalhos* que contém a ETag, pedir ID, tipo de conteúdo e codificação de conteúdo
* Um JSON *corpo* no seguinte formato:

   ```   {
       "status" : 201,
       "payload" : {...}
   }
   ```

   Ambos `status` e `body` são fornecidas pelo dispositivo e utilizados para responder com o código de estado do dispositivo e/ou a descrição.

## <a name="handle-a-direct-method-on-a-device"></a>Identificador de um método direto num dispositivo
### <a name="mqtt"></a>MQTT
#### <a name="method-invocation"></a>Invocação de métodos
Dispositivos recebem pedidos de método direto no tópico MQTT:`$iothub/methods/POST/{method name}/?$rid={request id}`

O corpo que recebe o dispositivo é o seguinte formato:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Pedidos de método são QoS 0.

#### <a name="response"></a>Resposta
O dispositivo envia as respostas `$iothub/methods/res/{status}/?$rid={request id}`, em que:

* O `status` propriedade é o estado fornecido pelo dispositivo de execução do método.
* O `$rid` propriedade é o ID do pedido de invocação do método recebido a partir do IoT Hub.

O corpo é definido pelo dispositivo e pode ser qualquer Estado.

### <a name="amqp"></a>AMQP
#### <a name="method-invocation"></a>Invocação de métodos
O dispositivo recebe pedidos de método direto através da criação de uma ligação de recepção no endereço`amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

A mensagem AMQP chega na ligação receive que representa o pedido de método. Contém o seguinte:
* A propriedade de ID de correlação, que contém um ID de pedido que deve ser transmitido novamente com a resposta de método correspondente
* Uma propriedade de aplicação com o nome `IoThub-methodname`, que contém o nome do método que está a ser invocado
* O corpo da mensagem AMQP que contém o payload de método como JSON

#### <a name="response"></a>Resposta
O dispositivo cria uma ligação de envio para devolver a resposta de método no endereço`amqps://{hostname}:5671/devices/{deviceId}/methods/deviceBound`

Resposta do método é devolvida na ligação de envio e está estruturada da seguinte forma:
* A propriedade de ID de correlação, que contém o ID do pedido transmitido na mensagem de pedido do método
* Uma propriedade de aplicação com o nome `IoThub-status`, que contém o utilizador a indicar o estado de método
* O corpo da mensagem AMQP que contém a resposta de método como JSON

## <a name="additional-reference-material"></a>Material de referência adicionais
Outros tópicos de referência no guia de programadores do IoT Hub incluem:

* [Pontos finais de IoT Hub] [ lnk-endpoints] descreve os vários pontos finais que cada IoT hub expõe para operações de gestão e de tempo de execução.
* [Limitação e quotas] [ lnk-quotas] descreve as quotas que se aplicam para o serviço de IoT Hub e o comportamento de limitação pode esperar quando utilizar o serviço.
* [Azure SDKs IoT do serviço e dispositivo] [ lnk-sdks] indica o idioma de vários SDKs que pode utilizar ao desenvolver aplicações de serviço e dispositivo que interagem com o IoT Hub.
* [Idioma de consulta do IoT Hub para dispositivos duplos, tarefas e o encaminhamento de mensagens] [ lnk-query] descreve o idioma de consulta do IoT Hub pode utilizar para obter informações a partir do IoT Hub sobre os dispositivos duplos e tarefas.
* [Suporte do IoT Hub MQTT] [ lnk-devguide-mqtt] fornece mais informações sobre o suporte do IoT Hub para o protocolo MQTT.

## <a name="next-steps"></a>Passos Seguintes
Agora que aprendeu como utilizar métodos diretos, poderá estar interessado no seguinte artigo de guia de programadores do IoT Hub:

* [Agenda de tarefas em vários dispositivos][lnk-devguide-jobs]

Se pretender experimentar alguns dos conceitos descritos neste artigo, poderá estar interessado no tutorial seguinte do IoT Hub:

* [Utilize métodos diretos][lnk-methods-tutorial]

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
