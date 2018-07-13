---
title: Exportar os dados no Azure IoT Central | Documentos da Microsoft
description: Como exportar dados a partir da sua aplicação do Azure IoT Central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/3/2018
ms.topic: article
ms.prod: azure-iot-central
manager: peterpr
ms.openlocfilehash: 6d35e3cfefcefef0b4ff40364cbdab92d486b769
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008812"
---
# <a name="export-your-data-in-azure-iot-central"></a>Exportar os dados no Azure IoT Central

Utilize a exportação contínua de dados para exportar periodicamente os dados na sua conta de armazenamento de Blobs do Azure. Optar pela exportação **medidas**, **dispositivos**, e **modelos de dispositivos** nos arquivos de [Apache AVRO](https://avro.apache.org/docs/current/index.html) formato. Utilize os dados exportados para a análise de caminho típico, tais como modelos de formação no Azure Machine Learning ou análise de tendências de longo prazo no Power BI.

> [!Note]
> Quando ativar a exportação contínua de dados, obtém apenas os dados que chega a partir desse momento e posteriores. Atualmente, não existe nenhuma forma de recuperar dados de quando a exportação contínua de dados foi desativada. Ative dados exportação contínua no início para manter os dados históricos mais!

## <a name="prerequisites"></a>Pré-requisitos

- Aplicação de avaliação de 30 dias estendida ou um aplicativo pago
- Conta do Azure com a subscrição do Azure
- A mesma conta do Azure é um administrador na sua aplicação do Centro de IoT
- A mesma conta do Azure tem permissões para criar uma conta de armazenamento ou aceder a uma conta de armazenamento existente na mesma subscrição do Azure

## <a name="types-of-data-to-export"></a>Tipos de dados para exportar

### <a name="measurements"></a>Medições

As medidas que os dispositivos enviam exportadas para a sua conta de armazenamento. Dados de medidas são exportados aproximadamente um minuto, que contém todas as novas mensagens que foram recebidas pelo centro de IoT de todos os dispositivos dentro daquela janela de tempo. Os ficheiros exportados do AVRO estão no mesmo formato que os ficheiros de mensagens exportados pelo [roteamento de mensagens do IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) para armazenamento de Blobs.

> [!NOTE]
> Os dispositivos que são enviadas as medidas são representados por identificações de dispositivo (ver abaixo). Para obter os nomes dos dispositivos, terá de exportar demasiado os instantâneos de dispositivos. Pode correlacionar cada registo de mensagens usando o connectionDeviceId que corresponde ao id do dispositivo.

Este é um exemplo de um registo no ficheiro AVRO decodificado.

```json
{
    "EnqueuedTimeUtc": "2018-06-11T00:00:08.2250000Z",
    "Properties": {},
    "SystemProperties": {
        "connectionDeviceId": "2383d8ba-c98c-403a-b4d5-8963859643bb",
        "connectionAuthMethod": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
        "connectionDeviceGenerationId": "636614021491644195",
        "enqueuedTime": "2018-06-11T00:00:08.2250000Z"
    },
    "Body": "{\"humidity\":80.59100954598546,\"magnetometerX\":0.29451796907056726,\"magnetometerY\":0.5550332126050068,\"magnetometerZ\":-0.04116681874733441,\"connectivity\":\"connected\",\"opened\":\"triggered\"}"
}
```

### <a name="devices"></a>Dispositivos

Quando ativar primeiro a exportação contínua de dados, um único instantâneo que contém todos os dispositivos é exportado. Isto inclui:
- Identificações de dispositivo
- Nomes de dispositivo
- IDs de modelo do dispositivo
- Valores de propriedades
- Valores de definições

Aproximadamente um minuto, um novo instantâneo é escrito que contém:

- Os novos dispositivos que foram adicionados desde o último instantâneo
- Dispositivos que tinham valores de propriedades e definições foram alterados desde o último instantâneo

> [!NOTE]
> Dispositivos que foram eliminados desde o último instantâneo não são exportados. Não existe nenhum indicador nos instantâneos para os dispositivos que foram eliminados neste momento.

> [!NOTE]
> O modelo de dispositivo que cada dispositivo pertence à é representado por um ID de modelo do dispositivo. Para obter o nome do modelo de dispositivo, terá de exportar os instantâneos de modelo do dispositivo demasiado.

Cada registo no ficheiro AVRO descodificado tem esta aparência:

```json
{
    "id": "2383d8ba-c98c-403a-b4d5-8963859643bb",
    "name": "Refrigerator 2",
    "simulated": true,
    "deviceTemplate": {
        "id": "c318d580-39fc-4aca-b995-843719821049",
        "version": "1.0.0"
    },
    "properties": {
        "cloud": {
            "location": "New York",
            "maintCon": true,
            "tempThresh": 20
        },
        "device": {
            "lastReboot": "2018-02-09T22:22:47.156Z"
        }
    },
    "settings": {
        "device": {
            "fanSpeed": 0
        }
    }
}
```

### <a name="device-templates"></a>Modelos de dispositivos

Quando ativar primeiro a exportação contínua de dados, um único instantâneo que contém todos os modelos de dispositivos é exportado. Isto inclui: 
- IDs de modelo do dispositivo
- Tipos de dados de medição e valores mínimos/máximos
- Tipos de dados de propriedades e valores predefinidos
- Tipos de dados de definições e valores predefinidos

Aproximadamente um minuto, um novo instantâneo é escrito que contém:

- Os novos modelos de dispositivos que foram adicionados desde o último instantâneo
- Modelos de dispositivos que tinha medidas, propriedades e definições de configurações que foram alterados desde o último instantâneo

> [!NOTE]
> Modelos de dispositivos que foram eliminados desde o último instantâneo não são exportados. Não há indicador de instantâneos para modelos de dispositivos que foram eliminados neste momento.

Cada registo no ficheiro AVRO descodificado tem esta aparência:

```json
{
    "id": "c318d580-39fc-4aca-b995-843719821049",
    "name": "Refrigerated Vending Machine",
    "version": "1.0.0",
    "measurements": {
        "telemetry": {
            "humidity": {
                "dataType": "double",
                "name": "Humidity"
            },
            "magnetometerX": {
                "dataType": "double",
                "name": "Magnetometer X"
            },
            "magnetometerY": {
                "dataType": "double",
                "name": "Magnetometer Y"
            },
            "magnetometerZ": {
                "dataType": "double",
                "name": "Magnetometer Z"
            }
        },
        "states": {
            "connectivity": {
                "dataType": "enum",
                "name": "Connectivity"
            }
        },
        "events": {
            "opened": {
                "name": "Door Opened",
                "category": "informational"
            }
        }
    },
    "settings": {
        "device": {
            "fanSpeed": {
                "dataType": "double",
                "name": "Fan Speed",
                "initialValue": 0
            }
        }
    },
    "properties": {
        "cloud": {
            "location": {
                "dataType": "string",
                "name": "Location",
                "initialValue": "Seattle"
            },
            "maintCon": {
                "dataType": "boolean",
                "name": "Maintenance Contract",
                "initialValue": true
            },
            "tempThresh": {
                "dataType": "double",
                "name": "Temperature Alert Threshold",
                "initialValue": 30
            }
        },
        "device": {
            "lastReboot": {
                "dataType": "dateTime",
                "name": "Last Reboot"
            }
        }
    }
}
```

## <a name="how-to-set-up-data-export"></a>Como configurar a exportação de dados

1. Se ainda não tiver uma, crie uma conta de armazenamento do Azure **na subscrição do Azure que a aplicação fica no**. [Clique aqui](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) para saltar para o portal do Azure para criar uma nova conta de armazenamento do Azure.

    - Escolher *fins gerais* ou *armazenamento de BLOBs* tipos de conta
    - Selecione a subscrição que a aplicação de IoT Central está no. Se não vir a subscrição, terá de iniciar sessão numa conta do Azure diferente ou pedir acesso à subscrição.
    - Pode escolher um grupo de recursos existente ou crie um novo. Saiba mais sobre [como criar uma nova conta de armazenamento.](https://aka.ms/blobdocscreatestorageaccount)

2. Crie um contentor na sua conta de armazenamento para exportar os dados de IoT Central. Vá para a sua conta de armazenamento -> Procurar Blobs e criar um novo contentor. ![Criar uma imagem de contentor](media/howto-export-data/createcontainer.png)

3. Inicie sessão na sua aplicação do Centro de IoT com a mesma conta do Azure.
1. Vá para a administração -> exportação contínua de dados.
![Centro de IoT CDE](media/howto-export-data/continuousdataexport.PNG)
1. Utilizar as listas pendentes, escolha a sua conta de armazenamento e um contentor. Em seguida, utilize os botões de alternar para ativar ou desativar os diferentes tipos de dados para exportar.
1. Por fim, ativar a exportação de dados contínua com o botão de alternar e clique em "Save".
1. Aguarde alguns minutos e deverá ver os dados são apresentados na sua conta de armazenamento. Pode navegar para a sua conta de armazenamento, selecione procurar blobs, selecione o seu contentor, e verá três pastas. Os caminhos predefinidos para os ficheiros AVRO que contém os diferentes tipos de dados são:
- Mensagens de: **{container}/measurements/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**
- Dispositivos: **{container}/devices/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**
- Modelos de dispositivos: **{container}/deviceTemplates/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**

## <a name="how-to-read-exported-avro-files"></a>Como ler exportados ficheiros AVRO

AVRO é um formato binário, para que os ficheiros não não possível ler no respetivo estado não processado. Eles podem ser descodificados no formato JSON. Os exemplos seguintes mostram como analisar as medidas, dispositivos e os modelos de dispositivo ficheiros do AVRO com os exemplos acima.

## <a name="python"></a>Python

### <a name="install-pandas-and-the-pandaavro-package"></a>Instale, Pandas e o pacote de PandaAvro:

```python
pip install pandas
pip install pandavro
```
### <a name="parse-measurements-avro-file"></a>Analisar o ficheiro AVRO medidas

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    measurements = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The SystemProperties column contains a dictionary with the device id
    # located under the "connectionDeviceId" key.
    transformed["device_id"] = measurements["SystemProperties"].apply(lambda x: x["connectionDeviceId"])

    # The Body column is a series of utf-8 bytes that is stringified and parsed
    # as json. In this example, we pull the "humidity" property off of each one
    # to get the humidity field.
    transformed["humidity"] = measurements["Body"].apply(lambda x: json.loads(bytes(x).decode('utf-8'))["humidity"])

    # Finally, we print the new DataFrame with our device ids and humidities
    print(transformed)

```
### <a name="parse-devices-avro-file"></a>Analisar o ficheiro AVRO de dispositivos

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    devices = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device id is available directly in the "id" column
    transformed["device_id"] = devices["id"]

    # The template id and version are present in a dictionary under the
    # deviceTemplate column
    transformed["template_id"] = devices["deviceTemplate"].apply(lambda x: x["id"])
    transformed["template_version"] = devices["deviceTemplate"].apply(lambda x: x["version"])

    # The fanSpeed setting value is located in a nested dictionary under the
    # settings column
    transformed["fan_speed"] = devices["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, we print the new DataFrame with our device and template
    # information, along with the value of the fan speed
    print(transformed)

```

### <a name="parse-device-templates-avro-file"></a>Analisar o ficheiro de AVRO de modelos de dispositivo

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    templates = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The template and version are available directly in the "id" and "version"
    # columns, respectively
    transformed["template_id"] = templates["id"]
    transformed["template_version"] = templates["version"]

    # The fanSpeed setting value is located in a nested dictionary under the
    # settings column
    transformed["fan_speed"] = templates["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, we print the new DataFrame with our device and template
    # information, along with the value of the fan speed
    print(transformed)
```

## <a name="c"></a>C#

### <a name="install-microsofthadoopavro"></a>Instalar Microsoft.Hadoop.Avro

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

### <a name="parse-measurements-avro-file"></a>Analisar o ficheiro AVRO medidas

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;
using Newtonsoft.Json;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contain multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the fields
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    var systemProperties = record.GetField<IDictionary<string, object>>("SystemProperties");
                    var deviceId = systemProperties["connectionDeviceId"] as string;
                    Console.WriteLine("Device ID: {0}", deviceId);

                    using (var stream = new MemoryStream(record.GetField<byte[]>("Body")))
                    {
                        using (var streamReader = new StreamReader(stream, Encoding.UTF8))
                        {
                            var body = JsonSerializer.Create().Deserialize(streamReader, typeof(IDictionary<string, dynamic>)) as IDictionary<string, dynamic>;
                            var humidity = body["humidity"];
                            Console.WriteLine("Humidity: {0}", humidity);
                        }
                    }
                }
            }
        }
    }
}
```

### <a name="parse-devices-avro-file"></a>Analisar o ficheiro AVRO de dispositivos

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contains multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the
                // fields from it
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // record and make the function IEnumerable<AvroRecord>
                    var deviceId = record.GetField<string>("id");

                    // The device template information is stored in a sub-record
                    // under the "deviceTemplate" field.
                    var deviceTemplateRecord = record.GetField<AvroRecord>("deviceTemplate");
                    var templateId = deviceTemplateRecord.GetField<string>("id");
                    var templateVersion = deviceTemplateRecord.GetField<string>("version");

                    // The settings and properties are nested two levels deep,
                    // with the first level indicating settings or properties
                    // and the second indicating the kind of setting/property
                    var settingsRecord = record.GetField<AvroRecord>("settings");
                    var deviceSettingsRecord = settingsRecord.GetField<IDictionary<string, dynamic>>("device");
                    var fanSpeed = deviceSettingsRecord["fanSpeed"];
                    
                    Console.WriteLine(
                        "ID: {0}, Template ID: {1}, Template Version: {2}, Fan Speed: {3}",
                        deviceId,
                        templateId,
                        templateVersion,
                        fanSpeed
                    );
                }
            }
        }
    }
}

```
### <a name="parse-device-templates-avro-file"></a>Analisar o ficheiro de AVRO de modelos de dispositivo

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contains multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the
                // fields from it
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // record and make the function IEnumerable<AvroRecord>
                    var id = record.GetField<string>("id");
                    var version = record.GetField<string>("version");

                    // The settings and properties are nested two levels deep,
                    // with the first level indicating settings or properties
                    // and the second indicating the kind of setting/property
                    var settingsRecord = record.GetField<AvroRecord>("settings");
                    var deviceSettingsRecord = settingsRecord.GetField<IDictionary<string, dynamic>>("device");
                    var fanSpeed = deviceSettingsRecord["fanSpeed"];
                    
                    Console.WriteLine(
                        "ID: {1}, Version: {2}, Fan Speed: {3}",
                        id,
                        version,
                        fanSpeed
                    );
                }
            }
        }
    }
}
```

## <a name="javascript"></a>Javascript

### <a name="install-avsc"></a>Instalar avsc

```javascript
npm install avsc
```

### <a name="parse-measurements-avro-file"></a>Analisar o ficheiro AVRO medidas

```javascript
const avro = require('avsc');

// Read the avro file and parse the device id and humidity from each record
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device id from the system properties
        const deviceId = record.SystemProperties.connectionDeviceId;

        // Convert the Body from a Buffer to a string and parse it
        const body = JSON.parse(record.Body.toString());

        // Get the humidty property off of the body
        const humidity = body.humidity;

        // Log the device id and humidity we found
        console.log(`Device ID: ${deviceId}`);
        console.log(`Humidity: ${humidity}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

### <a name="parse-devices-avro-file"></a>Analisar o ficheiro AVRO de dispositivos

```javascript
const avro = require('avsc');

// Read the avro file and parse the device and template identification info as
// well as the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device id from the id property
        const deviceId = record.id;

        // Fetch the tempalte id and version from the deviceTemplate property
        const deviceTemplateId = record.deviceTemplate.id;
        const deviceTemplateVersion = record.deviceTemplate.version;

        // Get the fanSpeed off the nested device settings property
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the device id and humidity we found
        console.log(`ID: ${deviceId}, Template ID: ${deviceTemplateId}, Template Version: ${deviceTemplateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

### <a name="parse-device-templates-avro-file"></a>Analisar o ficheiro de AVRO de modelos de dispositivo

```javascript
const avro = require('avsc');

// Read the avro file and parse the device and template identification info as
// well as the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the template id and version from the id and verison properties
        const templateId = record.id;
        const templateVersion = record.version;

        // Get the fanSpeed off the nested device settings property
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the device id and humidity we found
        console.log(`Template ID: ${templateId}, Template Version: ${templateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba como [gerir os seus dispositivos](howto-manage-devices.md) no Explorador do dispositivo. 
