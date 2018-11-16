---
title: Como utilizar funções definidas pelo utilizador no duplos Digital do Azure | Documentos da Microsoft
description: Orientação sobre como criar funções definidas pelo utilizador e as atribuições de funções matchers duplos Digital do Azure.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: alinast
ms.openlocfilehash: 6a757dca48dc3ff41adfe6f8802fad40e7a4ca81
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636837"
---
# <a name="how-to-use-user-defined-functions-in-azure-digital-twins"></a>Como utilizar funções definidas pelo utilizador no duplos Digital do Azure

[Funções definidas pelo utilizador](./concepts-user-defined-functions.md) enable de (UDF) o utilizador para executar a lógica personalizada em relação a mensagens de telemetria de entrada e os metadados de gráfico espaciais. Em seguida, o utilizador pode enviar eventos para pontos finais predefinidos. Este guia explica um exemplo de agir sobre eventos de temperatura para detetar e alerta sobre qualquer leitura que exceder uma determinada temperatura.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="client-library-reference"></a>Referência da biblioteca de cliente

As funções que estão disponíveis como métodos auxiliares no tempo de execução de funções definidas pelo utilizador são listadas na [referência de cliente](#Client-Reference) secção.

## <a name="create-a-matcher"></a>Criar na ferramenta de correspondência

Matchers são objetos de gráfico que determinam o que executar funções definidas pelo utilizador para uma mensagem de telemetria de determinado.

- Comparações de condição de válido na ferramenta de correspondência:

  - `Equals`
  - `NotEquals`
  - `Contains`

- Destinos de condição de válido na ferramenta de correspondência:

  - `Sensor`
  - `SensorDevice`
  - `SensorSpace`

A seguinte na ferramenta de correspondência do exemplo é avaliada como true em qualquer evento de telemetria de sensores com `"Temperature"` como seu valor de tipo de dados. Pode criar vários matchers numa função definida pelo utilizador:

```plaintext
POST YOUR_MANAGEMENT_API_URL/matchers
{
  "Name": "Temperature Matcher",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    }
  ],
  "SpaceId": "YOUR_SPACE_IDENTIFIER"
}
```

| Valor | Substituir |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | Qual sua instância estiver alojada num servidor a região |

## <a name="create-a-user-defined-function-udf"></a>Criar uma função definida pelo utilizador (UDF)

Depois dos matchers são criados, carregar o trecho de código de função com o seguinte procedimento **POST** chamar:

> [!IMPORTANT]
> - Nos cabeçalhos, defina o seguinte `Content-Type: multipart/form-data; boundary="userDefinedBoundary"`.
> - O corpo é com várias partes:
>   - A primeira parte é sobre metadados necessários para o UDF.
>   - A segunda parte é a lógica de computação do JavaScript.
> - Na **USER_DEFINED_BOUNDARY** secção, substitua a **SpaceId** e **Machers** valores.

```plaintext
POST YOUR_MANAGEMENT_API_URL/userdefinedfunctions with Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"
```

| Valor do parâmetro | Substituir |
| --- | --- |
| *USER_DEFINED_BOUNDARY* | Um nome de limites com várias partes de conteúdo |

### <a name="body"></a>Corpo

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "SpaceId": "YOUR_SPACE_IDENTIFIER",
  "Name": "User Defined Function",
  "Description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "Matchers": ["YOUR_MATCHER_IDENTIFIER"]
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--USER_DEFINED_BOUNDARY--
```

| Valor | Substituir |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | O identificador de espaço  |
| YOUR_MATCHER_IDENTIFIER | O ID da na ferramenta de correspondência que pretende utilizar |

### <a name="example-functions"></a>Funções de exemplo

Definir a telemetria de sensores diretamente para o sensor de leitura com o tipo de dados **temperatura**, que é `sensor.DataType`:

```JavaScript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

O **telemetria** parâmetro expõe a **SensorId** e **mensagem** atributos, correspondente a uma mensagem enviada por um sensor. O **executionContext** parâmetro expõe os seguintes atributos:

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

No exemplo seguinte, vamos registar uma mensagem se a leitura de telemetria de sensores excede um limiar predefinido. Se as definições de diagnóstico estão ativadas na instância duplos Digital do Azure, também são reencaminhados a registos de funções definidas pelo utilizador:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

O código a seguir aciona uma notificação se o nível de temperatura fica acima da constante predefinida:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Define threshold
  var threshold = 70;

  // Trigger notification 
  if(parseInt(parseReading) > threshold) {
    var alert = {
      message: 'Temperature reading has surpassed threshold',
      sensorId: telemetry.SensorId,
      reading: parseReading
    };

    sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(alert));
  }
}
```

Para um exemplo de código mais complexo de UDF [verificar espaços disponíveis com um ar fresco UDF](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js).

## <a name="create-a-role-assignment"></a>Criar uma atribuição de função

É necessário criar uma atribuição de função para a função definida pelo utilizador ser executado sob. Se não o fizermos, ele não tem as permissões adequadas para interagir com a API de gestão para efetuar ações em objetos de gráfico. As ações que executa a função definida pelo utilizador não são excluídas do controlo de acesso baseado em funções dentro das APIs de gestão de duplos Digital do Azure. Eles podem ter escopo limitados ao especificar determinadas funções ou determinados caminhos de controle de acesso. Para obter mais informações, consulte [controlo de acesso baseado em funções](./security-role-based-access-control.md) documentação.

1. Para funções de consulta e obter o ID da função que pretende atribuir para o UDF. Passá-lo para **RoleId**:

    ```plaintext
    GET YOUR_MANAGEMENT_API_URL/system/roles
    ```

1. **ObjectId** será o ID de UDF criado anteriormente.
1. Encontrar o valor de **caminho** consultando seus espaços com `fullpath`.
1. Copiar retornado `spacePaths` valor. Usará que no código a seguir:

    ```plaintext
    GET YOUR_MANAGEMENT_API_URL/spaces?name=YOUR_SPACE_NAME&includes=fullpath
    ```

    | Valor do parâmetro | Substituir |
    | --- | --- |
    | *YOUR_SPACE_NAME* | O nome do espaço de que pretende utilizar |

1. Cole retornado `spacePaths` valor para **caminho** para criar uma atribuição de função UDF:

    ```plaintext
    POST YOUR_MANAGEMENT_API_URL/roleassignments
    {
      "RoleId": "YOUR_DESIRED_ROLE_IDENTIFIER",
      "ObjectId": "YOUR_USER_DEFINED_FUNCTION_ID",
      "ObjectIdType": "YOUR_USER_DEFINED_FUNCTION_TYPE_ID",
      "Path": "YOUR_ACCESS_CONTROL_PATH"
    }
    ```

    | O valor | Substituir |
    | --- | --- |
    | YOUR_DESIRED_ROLE_IDENTIFIER | O identificador para a função desejada |
    | YOUR_USER_DEFINED_FUNCTION_ID | O ID para o UDF que pretende utilizar |
    | YOUR_USER_DEFINED_FUNCTION_TYPE_ID | O ID de especificar o tipo UDF |
    | YOUR_ACCESS_CONTROL_PATH | O caminho de controlo de acesso |

## <a name="send-telemetry-to-be-processed"></a>Enviar telemetria para serem processados

Telemetria gerada pelo sensor descrito no gráfico aciona a execução da função definida pelo utilizador que foi carregada. O processador dos dados seleciona a telemetria. Em seguida, um plano de execução é criado para a invocação da função definida pelo utilizador.

1. Obter os matchers para o sensor que a leitura foi gerada fora do.
1. Consoante o que matchers avaliada com êxito, obter as funções definidas pelo utilizador associadas.
1. Execute cada função definida pelo utilizador.

## <a name="client-reference"></a>Referência de cliente

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Tendo em conta um identificador de espaço, esta função recupera o espaço do gráfico.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Identificador de espaço |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Tendo em conta um identificador de sensor, essa função obtém o sensor do gráfico.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ---------- | ------------------- | ------------ |
| *id*  | `guid` | Identificador de sensor |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Tendo em conta um identificador de dispositivo, esta função recupera o dispositivo do gráfico.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *id* | `guid` | Identificador de dispositivo |

### <a name="getsensorvaluesensorid-datatype--value"></a>⇒ getSensorValue (sensorId, tipo de dados) `value`

Tendo em conta um identificador de sensor e o respetivo tipo de dados, essa função obtém o valor atual para esse sensor.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *sensorId*  | `guid` | Identificador de sensor |
| *Tipo de dados*  | `string` | tipo de dados do sensor |

### <a name="getspacevaluespaceid-valuename--value"></a>⇒ getSpaceValue (spaceId, valueName) `value`

Com um identificador de espaço e o nome do valor, esta função recupera o valor atual para essa propriedade de espaço.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId*  | `guid` | Identificador de espaço |
| *valueName* | `string` | nome da propriedade de espaço |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>⇒ getSensorHistoryValues (sensorId, tipo de dados) `value[]`

Tendo em conta um identificador de sensor e o respetivo tipo de dados, esta função recupera os valores históricos para esse sensor.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador de sensor |
| *Tipo de dados* | `string` | tipo de dados do sensor |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>⇒ getSpaceHistoryValues (spaceId, tipo de dados) `value[]`

Com um identificador de espaço e o nome do valor, esta função recupera os valores históricos para essa propriedade no espaço de.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |
| *valueName* | `string` | nome da propriedade de espaço |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Tendo em conta um identificador de espaço, essa função obtém os espaços de subordinados para esse espaço principal.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Tendo em conta um identificador de espaço, essa função obtém os sensores de subordinados para esse espaço principal.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Tendo em conta um identificador de espaço, essa função obtém os dispositivos de subordinados para esse espaço principal.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Tendo em conta um identificador de dispositivo, esta função recupera os sensores de subordinados para esse dispositivo principal.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Identificador de dispositivo |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Tendo em conta um identificador de espaço, essa função obtém seu espaço de principal.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *childSpaceId* | `guid` | Identificador de espaço |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Tendo em conta um identificador de sensor, essa função obtém seu espaço de principal.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Identificador de sensor |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Tendo em conta um identificador de dispositivo, essa função obtém seu espaço de principal.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *childDeviceId* | `guid` | Identificador de dispositivo |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) ⇒ `space`

Tendo em conta um identificador de sensor, essa função obtém o respetivo dispositivo principal.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *childSensorId* | `guid` | Identificador de sensor |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>⇒ getSpaceExtendedProperty (spaceId, propertyName) `extendedProperty`

Tendo em conta um identificador de espaço, esta função recupera a propriedade e o respetivo valor do espaço.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |
| *propertyName* | `string` | nome da propriedade de espaço |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>⇒ getSensorExtendedProperty (sensorId, propertyName) `extendedProperty`

Tendo em conta um identificador de sensor, esta função recupera a propriedade e o respetivo valor do sensor.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador de sensor |
| *propertyName* | `string` | nome de propriedade do sensor |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>⇒ getDeviceExtendedProperty (deviceId, propertyName) `extendedProperty`

Tendo em conta um identificador de dispositivo, esta função recupera a propriedade e o respetivo valor do dispositivo.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *deviceId* | `guid` | Identificador de dispositivo |
| *propertyName* | `string` | nome de propriedade do dispositivo |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue (sensorId, tipo de dados, valor)

Esta função define um valor no objeto de sensor com o tipo de dados fornecido.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *sensorId* | `guid` | Identificador de sensor |
| *Tipo de dados*  | `string` | tipo de dados do sensor |
| *valor*  | `string` | Valor |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue (spaceId, tipo de dados, valor)

Esta função define um valor no objeto espaço com o tipo de dados fornecido.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *spaceId* | `guid` | Identificador de espaço |
| *Tipo de dados* | `string` | Tipo de dados |
| *valor* | `string` | Valor |

### <a name="logmessage"></a>log(Message)

Esta função regista a mensagem seguinte dentro da função definida pelo utilizador.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *message* | `string` | mensagem de ter sessão iniciada |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification (topologyObjectId, topologyObjectType, payload)

Esta função envia uma notificação para ser expedida.

**Tipo**: função global

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *topologyObjectId*  | `guid` | Identificador de objeto de gráfico. Os exemplos são espaço, o sensor e o ID de dispositivo.|
| *topologyObjectType*  | `string` | Os exemplos são sensor e do dispositivo.|
| *Payload*  | `string` | O payload JSON para serem enviados com a notificação. |

## <a name="return-types"></a>Tipos de retorno

Os modelos seguintes descrevem os objetos de retornados de referência de cliente anterior.

### <a name="space"></a>Espaço

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000000",
  "Name": "Space",
  "FriendlyName": "Conference Room",
  "TypeId": 0,
  "ParentSpaceId": "00000000-0000-0000-0000-000000000001",
  "SubtypeId": 0
}
```

### <a name="space-methods"></a>Métodos de espaço

#### <a name="parent--space"></a>Parent() ⇒ `space`

Esta função devolve o espaço de principal do espaço atual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Essa função retorna o filho sensores do espaço atual.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Essa função retorna o filho de dispositivos do espaço atual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Essa função retorna a propriedade estendida e o respetivo valor para o atual espaço.

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | nome da propriedade expandida |

#### <a name="valuevaluename--value"></a>Value(VALUENAME) ⇒ `value`

Esta função devolve o valor do espaço atual.

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | nome do valor |

#### <a name="historyvaluename--value"></a>History(VALUENAME) ⇒ `value[]`

Esta função devolve os valores de histórico do espaço atual.

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *valueName* | `string` | nome do valor |

#### <a name="notifypayload"></a>NOTIFY(Payload)

Esta função envia uma notificação com o payload especificado.

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *Payload* | `string` | Payload JSON para incluir na notificação |

### <a name="device"></a>Dispositivo

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000002",
  "Name": "Device",
  "FriendlyName": "Temperature Sensing Device",
  "Description": "This device contains a sensor that captures temperature readings.",
  "Type": "None",
  "Subtype": "None",
  "TypeId": 0,
  "SubtypeId": 0,
  "HardwareId": "ABC123",
  "GatewayId": "ABC",
  "SpaceId": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="device-methods"></a>Métodos do dispositivo

#### <a name="parent--space"></a>Parent() ⇒ `space`

Esta função devolve o espaço de principal do dispositivo atual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Essa função retorna o filho sensores do dispositivo atual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Essa função retorna a propriedade estendida e o respetivo valor para o dispositivo atual.

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | nome da propriedade expandida |

#### <a name="notifypayload"></a>NOTIFY(Payload)

Esta função envia uma notificação com o payload especificado.

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *Payload* | `string` | Payload JSON para incluir na notificação |

### <a name="sensor"></a>Sensor

```JSON
{
  "Id": "00000000-0000-0000-0000-000000000003",
  "Port": "30",
  "PollRate": 3600,
  "DataType": "Temperature",
  "DataSubtype": "None",
  "Type": "Classic",
  "PortType": "None",
  "DataUnitType": "FahrenheitTemperature",
  "SpaceId": "00000000-0000-0000-0000-000000000000",
  "DeviceId": "00000000-0000-0000-0000-000000000001",
  "PortTypeId": 0,
  "DataUnitTypeId": 0,
  "DataTypeId": 0,
  "DataSubtypeId": 0,
  "TypeId": 0  
}
```

### <a name="sensor-methods"></a>Métodos de sensor

#### <a name="space--space"></a>Space() ⇒ `space`

Esta função devolve o espaço de principal do sensor de corrente.

#### <a name="device--device"></a>Device() ⇒ `device`

Esta função devolve o dispositivo principal do sensor de corrente.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Essa função retorna a propriedade estendida e o respetivo valor para o sensor de corrente.

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *propertyName* | `string` | nome da propriedade expandida |

#### <a name="value--value"></a>Value () ⇒ `value`

Esta função devolve o valor do sensor de corrente.

#### <a name="history--value"></a>History() ⇒ `value[]`

Esta função devolve os valores de histórico do sensor de corrente.

#### <a name="notifypayload"></a>NOTIFY(Payload)

Esta função envia uma notificação com o payload especificado.

| Parâmetro  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| *Payload* | `string` | Payload JSON para incluir na notificação |

### <a name="value"></a>Valor

```JSON
{
  "DataType": "Temperature",
  "Value": "70",
  "CreatedTime": ""
}
```

### <a name="extended-property"></a>Propriedade expandida

```JSON
{
  "Name": "OccupancyStatus",
  "Value": "Occupied"
}
```

## <a name="next-steps"></a>Passos Seguintes

- Saiba como [criar pontos finais de duplos Digital do Azure](how-to-egress-endpoints.md) para enviar eventos para.

- Para obter mais detalhes nos pontos finais de duplos Digital do Azure, aprender [mais informações sobre pontos finais](concepts-events-routing.md).
