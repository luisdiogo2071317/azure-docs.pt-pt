---
title: Como utilizar funções definidas pelo utilizador no duplos Digital do Azure | Documentos da Microsoft
description: Orientação sobre como criar funções definidas pelo utilizador, matchers e atribuições de funções duplos Digital do Azure.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 7fbaff5ed1b60a4434ba2eb0c78c6aa1f3fd6645
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324232"
---
# <a name="how-to-use-user-defined-functions-in-azure-digital-twins"></a>Como utilizar funções definidas pelo utilizador no duplos Digital do Azure

[Funções definidas pelo utilizador](./concepts-user-defined-functions.md) permitir que o utilizador executar a lógica personalizada em relação a mensagens de telemetria de entrada e de metadados de gráfico espaciais, permitindo que o usuário enviar eventos para pontos de extremidade previamente definidos. Neste guia, vamos examinar um exemplo de agir sobre eventos de temperatura para detetar e alerta sobre qualquer leitura que exceder uma determinada temperatura.

Nos exemplos abaixo, `https://yourManagementApiUrl` refere-se para o URI de APIs de duplos Digital:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| `yourInstanceName` | O nome da sua instância de duplos Digital do Azure |
| `yourLocation` | Qual sua instância estiver alojada num servidor a região |

## <a name="client-library-reference"></a>Referência da biblioteca de cliente

As funções que estão disponíveis como métodos auxiliares no tempo de execução de funções definidas pelo utilizador são enumeradas na seguinte [referência de cliente](#Client-Reference).

## <a name="create-a-matcher"></a>Criar na ferramenta de correspondência

Matchers são objetos de gráfico, que determinam quais funções definidas pelo usuário serão executadas de uma mensagem de telemetria de determinado.

Comparações de condição de válido na ferramenta de correspondência:

- `Equals`
- `NotEquals`
- `Contains`

Destinos de condição de válido na ferramenta de correspondência:

- `Sensor`
- `SensorDevice`
- `SensorSpace`

A seguinte na ferramenta de correspondência do exemplo será avaliado como true em qualquer evento de telemetria de sensores com `Temperature` como seu valor de tipo de dados. Pode criar vários matchers numa função definida pelo utilizador.

```text
POST https://yourManagementApiUrl/api/v1.0/matchers
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
  "SpaceId": "yourSpaceIdentifier"
}
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| `yourManagementApiUrl` | O caminho do URL completo para a API de gestão  |
| `yourSpaceIdentifier` | Qual sua instância estiver alojada num servidor a região |

## <a name="create-a-user-defined-function-udf"></a>Criar uma função definida pelo utilizador (UDF)

Depois dos matchers foram criados, carregue o trecho de código de função com a seguinte chamada POST:

> [!IMPORTANT]
> - Nos cabeçalhos, defina o seguinte `Content-Type: multipart/form-data; boundary="userDefinedBoundary"`.
> - O corpo é com várias parte:
>   - A primeira parte é sobre metadados necessários para o UDF.
>   - A segunda parte é a lógica de computação do javascript.
> - Substituir na `userDefinedBoundary` secção `SpaceId` e `Machers` Guids.

```plaintext
POST https://yourManagementApiUrl/api/v1.0/userdefinedfunctions with Content-Type: multipart/form-data; boundary="userDefinedBoundary"
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| `yourManagementApiUrl` | O caminho do URL completo para a API de gestão  |

Corpo:

```plaintext
--userDefinedBoundary
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "SpaceId": "yourSpaceIdentifier",
  "Name": "User Defined Function",
  "Description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "Matchers": ["yourMatcherIdentifier"]
}
--userDefinedBoundary
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--userDefinedBoundary--
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| `yourSpaceIdentifier` | O identificador de espaço  |
| `yourMatcherIdentifier` | O id na que vai utilizar a ferramenta de correspondência |

### <a name="example-functions"></a>Funções de exemplo

Definir a telemetria de sensores diretamente para o sensor de leitura com o tipo de dados `Temperature`, que é o sensor. Tipo de dados:

```javascript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

Registar uma mensagem se a leitura de telemetria de sensores excede um limiar predefinido. Se as definições de diagnóstico estão ativadas na instância duplos Digital, registos de funções definidas pelo utilizador serão reencaminhados:

```javascript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

O código seguinte irá acionar uma notificação se o nível de temperatura fica acima da constante predefinida.

```javascript
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

Para obter um exemplo de código UDF mais complexo, consulte [verificar disponíveis espaços com ar fresco UDF](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js)

## <a name="create-a-role-assignment"></a>Criar uma atribuição de função

É necessário criar uma atribuição de função para a função definida pelo utilizador ser executados no. Se não precisamos fazer isso, ele não terá as permissões adequadas para interagir com a API de gestão para efetuar ações em objetos de gráfico. As ações que executa a função definida pelo utilizador não são excluídas do controlo de acesso baseado em funções dentro das APIs de gestão de duplos Digital. Eles podem ter escopo limitados ao especificar determinadas funções ou determinados caminhos de controle de acesso. Para obter mais informações, consulte [controlo de acesso baseado em funções](./security-role-based-access-control.md) documentação.

- Para funções de consulta e obter o ID da função que pretende atribuir a UDF; transferi-los a RoleId abaixo.

```plaintext
GET https://yourManagementApiUrl/api/v1.0/system/roles
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| `yourManagementApiUrl` | O caminho do URL completo para a API de gestão  |

- ObjectId será o ID de UDF criado anteriormente
- Encontrar `Path` consultando os espaços por seu caminho completo e copie o `spacePaths` valor. Cole-o no caminho abaixo ao criar a atribuição de função UDF

```plaintext
GET https://yourManagementApiUrl/api/v1.0/spaces?name=yourSpaceName&includes=fullpath
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| `yourManagementApiUrl` | O caminho do URL completo para a API de gestão  |
| `yourSpaceName` | O nome do espaço de que pretende utilizar |

```plaintext
POST https://yourManagementApiUrl/api/v1.0/roleassignments
{
  "RoleId": "yourDesiredRoleIdentifier",
  "ObjectId": "yourUserDefinedFunctionId",
  "ObjectIdType": "UserDefinedFunctionId",
  "Path": "yourAccessControlPath"
}
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| `yourManagementApiUrl` | O caminho do URL completo para a API de gestão  |
| `yourDesiredRoleIdentifier` | O identificador para a função desejada |
| `yourUserDefinedFunctionId` | O id para o UDF que pretende utilizar |
| `yourAccessControlPath` | O caminho de controlo de acesso |

## <a name="send-telemetry-to-be-processed"></a>Enviar telemetria para serem processados

Telemetria gerada pelo sensor descrito no gráfico deve acionar a execução da função definida pelo utilizador que foi carregada. Assim que a telemetria é recolhida pelo processador dos dados, é criado um plano de execução para a invocação da função definida pelo utilizador.

1. Obter os matchers para o sensor que a leitura foi gerada fora do.
1. Consoante o que matchers avaliada com êxito, obter as funções definidas pelo utilizador associadas.
1. Execute cada função definida pelo utilizador.

## <a name="client-reference"></a>Referência de cliente

### <a name="getspacemetadataid--space"></a>getSpaceMetadata(id) ⇒ `space`

Fornecido identificador de espaço, obtém o espaço do gráfico.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| ID  | `guid` | Identificador de espaço |

### <a name="getsensormetadataid--sensor"></a>getSensorMetadata(id) ⇒ `sensor`

Fornecido identificador de sensor, obtém o sensor do gráfico.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| ID  | `guid` | Identificador de sensor |

### <a name="getdevicemetadataid--device"></a>getDeviceMetadata(id) ⇒ `device`

Fornecido identificador de dispositivo, obtém o dispositivo do gráfico.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| ID  | `guid` | Identificador de dispositivo |

### <a name="getsensorvaluesensorid-datatype--value"></a>⇒ getSensorValue (sensorId, tipo de dados) `value`

Tendo em conta um identificador de sensor e o respetivo tipo de dados, obter o valor atual para esse sensor.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Identificador de sensor |
| Tipo de dados  | `string` | tipo de dados do sensor |

### <a name="getspacevaluespaceid-valuename--value"></a>⇒ getSpaceValue (spaceId, valueName) `value`

Com um identificador de espaço e o nome do valor, recupere o valor atual para essa propriedade de espaço.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Identificador de espaço |
| valueName  | `string` | nome da propriedade de espaço |

### <a name="getsensorhistoryvaluessensorid-datatype--value"></a>⇒ getSensorHistoryValues (sensorId, tipo de dados) `value[]`

Tendo em conta um identificador de sensor e o respetivo tipo de dados, obter os valores históricos para esse sensor.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Identificador de sensor |
| Tipo de dados  | `string` | tipo de dados do sensor |

### <a name="getspacehistoryvaluesspaceid-datatype--value"></a>⇒ getSpaceHistoryValues (spaceId, tipo de dados) `value[]`

Com um identificador de espaço e o nome do valor, obter os valores históricos para essa propriedade no espaço de.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Identificador de espaço |
| valueName  | `string` | nome da propriedade de espaço |

### <a name="getspacechildspacesspaceid--space"></a>getSpaceChildSpaces(spaceId) ⇒ `space[]`

Tendo em conta um identificador de espaço, obter os espaços de subordinados para esse espaço principal.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Identificador de espaço |

### <a name="getspacechildsensorsspaceid--sensor"></a>getSpaceChildSensors(spaceId) ⇒ `sensor[]`

Tendo em conta um identificador de espaço, obter os sensores de subordinados para esse espaço principal.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Identificador de espaço |

### <a name="getspacechilddevicesspaceid--device"></a>getSpaceChildDevices(spaceId) ⇒ `device[]`

Tendo em conta um identificador de espaço, obter os dispositivos de subordinados para esse espaço principal.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Identificador de espaço |

### <a name="getdevicechildsensorsdeviceid--sensor"></a>getDeviceChildSensors(deviceId) ⇒ `sensor[]`

Tendo em conta um identificador de dispositivo, obter os sensores de subordinados para esse dispositivo principal.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| deviceId  | `guid` | Identificador de dispositivo |

### <a name="getspaceparentspacechildspaceid--space"></a>getSpaceParentSpace(childSpaceId) ⇒ `space`

Tendo em conta um identificador de espaço, obter o seu espaço de principal.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| childSpaceId  | `guid` | Identificador de espaço |

### <a name="getsensorparentspacechildsensorid--space"></a>getSensorParentSpace(childSensorId) ⇒ `space`

Tendo em conta um identificador de sensor, obter o seu espaço de principal.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| childSensorId  | `guid` | Identificador de sensor |

### <a name="getdeviceparentspacechilddeviceid--space"></a>getDeviceParentSpace(childDeviceId) ⇒ `space`

Tendo em conta um identificador de dispositivo, obter o seu espaço de principal.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| childDeviceId  | `guid` | Identificador de dispositivo |

### <a name="getsensorparentdevicechildsensorid--space"></a>getSensorParentDevice(childSensorId) ⇒ `space`

Tendo em conta um identificador de sensor, obter o seu dispositivo principal.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| childSensorId  | `guid` | Identificador de sensor |

### <a name="getspaceextendedpropertyspaceid-propertyname--extendedproperty"></a>⇒ getSpaceExtendedProperty (spaceId, propertyName) `extendedProperty`

Tendo em conta um identificador de espaço, obter a propriedade e o respetivo valor do espaço.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Identificador de espaço |
| propertyName  | `string` | nome da propriedade de espaço |

### <a name="getsensorextendedpropertysensorid-propertyname--extendedproperty"></a>⇒ getSensorExtendedProperty (sensorId, propertyName) `extendedProperty`

Tendo em conta um identificador de sensor, obter a propriedade e o respetivo valor do sensor.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Identificador de sensor |
| propertyName  | `string` | nome de propriedade do sensor |

### <a name="getdeviceextendedpropertydeviceid-propertyname--extendedproperty"></a>⇒ getDeviceExtendedProperty (deviceId, propertyName) `extendedProperty`

Tendo em conta um identificador de dispositivo, obter a propriedade e o respetivo valor do dispositivo.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| deviceId  | `guid` | Identificador de dispositivo |
| propertyName  | `string` | nome de propriedade do dispositivo |

### <a name="setsensorvaluesensorid-datatype-value"></a>setSensorValue (sensorId, tipo de dados, valor)

Define um valor no objeto de sensor com o tipo de dados fornecido.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| sensorId  | `guid` | Identificador de sensor |
| Tipo de dados  | `string` | tipo de dados do sensor |
| valor  | `string` | valor |

### <a name="setspacevaluespaceid-datatype-value"></a>setSpaceValue (spaceId, tipo de dados, valor)

Define um valor no objeto espaço com o tipo de dados fornecido.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| spaceId  | `guid` | Identificador de espaço |
| Tipo de dados  | `string` | tipo de dados |
| valor  | `string` | valor |

### <a name="logmessage"></a>log(Message)

Regista a mensagem seguinte dentro da função definida pelo utilizador.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| message  | `string` | mensagem de ter sessão iniciada |

### <a name="sendnotificationtopologyobjectid-topologyobjecttype-payload"></a>sendNotification (topologyObjectId, topologyObjectType, payload)

Envia uma notificação para ser expedida.

**Tipo**: função global

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| topologyObjectId  | `Guid` | criar um gráfico de identificador de objeto (ex. espaço / id do sensor /device)|
| topologyObjectType  | `string` | (ex. espaço / sensor / dispositivo)|
| payload  | `string` | o payload de json para serem enviados com a notificação |

## <a name="return-types"></a>Tipos de retorno

Seguem-se os modelos que descrevem os objetos de retornados de referência de cliente acima.

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

Devolve o espaço de principal do espaço atual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Devolve o subordinado sensores do espaço atual.

#### <a name="childdevices--device"></a>ChildDevices() ⇒ `device[]`

Devolve o subordinado de dispositivos do espaço atual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Devolve a propriedade estendida e o respetivo valor para o atual espaço.

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| propertyName | `string` | nome da propriedade expandida |

#### <a name="valuevaluename--value"></a>Value(VALUENAME) ⇒ `value`

Devolve o valor do espaço atual.

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| valueName | `string` | nome do valor |

#### <a name="historyvaluename--value"></a>History(VALUENAME) ⇒ `value[]`

Devolve os valores de histórico do espaço atual.

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| valueName | `string` | nome do valor |

#### <a name="notifypayload"></a>NOTIFY(Payload)

Envia uma notificação com o payload especificado.

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| payload | `string` | payload de JSON para incluir na notificação |

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

### <a name="device-methods"></a>Métodos de dispositivo

#### <a name="parent--space"></a>Parent() ⇒ `space`

Devolve o espaço de principal do dispositivo atual.

#### <a name="childsensors--sensor"></a>ChildSensors() ⇒ `sensor[]`

Devolve o subordinado sensores do dispositivo atual.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Devolve a propriedade estendida e o respetivo valor para o dispositivo atual.

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| propertyName | `string` | nome da propriedade expandida |

#### <a name="notifypayload"></a>NOTIFY(Payload)

Envia uma notificação com o payload especificado.

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| payload | `string` | payload de JSON para incluir na notificação |

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

Devolve o espaço de principal do sensor de corrente.

#### <a name="device--device"></a>Device() ⇒ `device`

Devolve o dispositivo principal do sensor de corrente.

#### <a name="extendedpropertypropertyname--extendedproperty"></a>ExtendedProperty(propertyName) ⇒ `extendedProperty`

Devolve a propriedade estendida e o respetivo valor para o sensor de corrente.

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| propertyName | `string` | nome da propriedade expandida |

#### <a name="value--value"></a>Value () ⇒ `value`

Devolve o valor do sensor de corrente.

#### <a name="history--value"></a>History() ⇒ `value[]`

Devolve os valores de histórico do sensor de corrente.

#### <a name="notifypayload"></a>NOTIFY(Payload)

Envia uma notificação com o payload especificado.

| Param  | Tipo                | Descrição  |
| ------ | ------------------- | ------------ |
| payload | `string` | payload de JSON para incluir na notificação |

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

Para saber como criar pontos finais de duplos Digital para enviar eventos para, leia [pontos de extremidade de criar Digital duplos](how-to-egress-endpoints.md).

Para obter mais detalhes sobre a pontos finais de duplos Digital, leia [Saiba mais sobre os pontos finais](concepts-events-routing.md).
