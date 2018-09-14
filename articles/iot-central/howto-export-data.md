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
ms.openlocfilehash: 5defbf7021936e3cc77250ccc453cb3887c77617
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576447"
---
# <a name="export-your-data-in-azure-iot-central"></a>Exportar os dados no Azure IoT Central

Este artigo descreve como utilizar a funcionalidade de exportação contínua de dados no Azure IoT Central para exportar periodicamente os dados à sua conta de armazenamento de Blobs do Azure. Pode exportar **medidas**, **dispositivos**, e **modelos de dispositivos** para ficheiros com o [Apache AVRO](https://avro.apache.org/docs/current/index.html) formato. Os dados exportados podem ser utilizados para análise de caminho típico, como modelos de formação no Azure Machine Learning ou análise de tendências de longo prazo no Microsoft Power BI.

> [!Note]
> Quando ativar a exportação contínua de dados, obtém apenas os dados a partir desse momento ou superior. Atualmente, não poderá obter dados para um tempo quando a exportação de dados contínua foi desativada. Para manter mais dados históricos, ative a exportação contínua de dados desde o início.

## <a name="prerequisites"></a>Pré-requisitos

- Um aplicativo expandido para o 30 dias avaliação IoT Central, ou uma aplicação paga.
- Uma conta do Azure com uma subscrição do Azure.
- A mesma conta do Azure é um administrador na sua aplicação IoT Central.
- A mesma conta do Azure tem permissões para criar uma conta de armazenamento ou aceder a uma conta de armazenamento existente na mesma subscrição do Azure.

## <a name="types-of-data-to-export"></a>Tipos de dados para exportar

### <a name="measurements"></a>Medições

As medidas que os dispositivos enviam são exportadas para a sua conta de armazenamento, uma vez por minuto. Os dados têm novas mensagens recebidas pelo centro de IoT de todos os dispositivos, durante esse período. Os ficheiros exportados do AVRO utilizam o mesmo formato que os ficheiros de mensagem exportados pelo [roteamento de mensagens do IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) para armazenamento de Blobs.

> [!NOTE]
> Os dispositivos que enviam as medidas são representados por identificações de dispositivo (consulte as seções a seguir). Para obter os nomes dos dispositivos, exporte os instantâneos de dispositivo. Correlacionar cada registo de mensagens utilizando o **connectionDeviceId** que corresponda ao ID de dispositivo.

O exemplo seguinte mostra um registo num ficheiro AVRO descodificado:

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

Quando a exportação contínua de dados pela primeira vez é ativada, um instantâneo único com todos os dispositivos é exportado. O instantâneo inclui:
- Identificações de dispositivo.
- Nomes de dispositivo.
- IDs de modelo de dispositivo.
- Valores de propriedade.
- Valores de definição.

Um novo instantâneo é escrito uma vez por minuto. O instantâneo inclui:

- Novos dispositivos adicionados desde o último instantâneo.
- Dispositivos com a propriedade alterada e definir valores desde o último instantâneo.

> [!NOTE]
> Dispositivos eliminados, uma vez que o último instantâneo não são exportadas. Atualmente, os instantâneos não tem indicadores para dispositivos eliminados.
>
> O modelo de dispositivo que cada dispositivo pertence à é representado por um ID de modelo do dispositivo. Para obter o nome do modelo de dispositivo, exporte os instantâneos de modelo do dispositivo.

Cada registo no ficheiro AVRO descodificado é semelhante a:

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

Quando a exportação contínua de dados pela primeira vez é ativada, um instantâneo único com todos os modelos de dispositivos é exportado. O instantâneo inclui: 
- IDs de modelo de dispositivo.
- Tipos de dados de medição e valores mínimos/máximos.
- Tipos de dados de propriedade e os valores predefinidos.
- Definir tipos de dados e valores predefinidos.

Um novo instantâneo é escrito uma vez por minuto. O instantâneo inclui:

- Novos modelos de dispositivo adicionados desde o último instantâneo.
- Modelos de dispositivos com medidas alteradas, propriedade e definir as definições de desde o último instantâneo.

> [!NOTE]
> Modelos de dispositivos eliminados desde o último instantâneo não são exportados. Atualmente, os instantâneos não tem indicadores para modelos de dispositivos eliminada.

Cada registo no ficheiro AVRO descodificado é semelhante a:

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

## <a name="set-up-continuous-data-export"></a>Configurar a exportação contínua de dados

1. Se não tiver uma conta de armazenamento do Azure, [criar uma nova conta de armazenamento](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) no portal do Azure. Criar a conta de armazenamento **na subscrição do Azure que tenha a sua aplicação do Centro de IoT**.
    - Para o tipo de conta, escolha **fins gerais** ou **armazenamento de BLOBs**.
    - Selecione a subscrição que tem a sua aplicação IoT Central. Se não vir a subscrição, poderá ter de iniciar sessão para um diferente do Azure conta ou para pedir acesso à subscrição.
    - Escolha um grupo de recursos existente ou crie um novo. Saiba mais sobre [como criar uma nova conta de armazenamento](https://aka.ms/blobdocscreatestorageaccount).

2. Crie um contentor na sua conta de armazenamento para exportar os dados de IoT Central. Aceda à sua conta de armazenamento. Sob **serviço Blob**, selecione **procurar Blobs**. Selecione **contentor** para criar um novo contentor.

   ![Criar um contentor](media/howto-export-data/createcontainer.png)

3. Inicie sessão para a sua aplicação do Centro de IoT com a mesma conta do Azure.

4. Sob **Administration**, selecione **exportar dados**.

   ![Configurar a exportação contínua de dados](media/howto-export-data/continuousdataexport.PNG)

5. Na **conta de armazenamento** na lista pendente caixa, selecione a sua conta de armazenamento. Na **contentor** na lista pendente caixa, selecione o seu contentor. Sob **dados para exportar**, especifique cada tipo de dados para exportar, definindo o tipo como **no**.

6. Para ativar a exportação contínua de dados, defina **exportar dados** ao **no**. Selecione **Guardar**.

7. Após alguns minutos, os seus dados é apresentado na sua conta de armazenamento. Navegue para a sua conta de armazenamento. Selecione **procurar blobs** > seu contentor. Verá três pastas para os dados de exportação. Os caminhos predefinidos para os ficheiros AVRO com os dados de exportação são:
    - Mensagens de: {container}/measurements/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro
    - Dispositivos: {container}/devices/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro
    - Modelos de dispositivos: {container}/deviceTemplates/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro

## <a name="read-exported-avro-files"></a>Leitura exportados ficheiros AVRO

AVRO é um formato binário, para que os ficheiros não não possível ler no respetivo estado não processado. Os ficheiros podem ser descodificados no formato JSON. Os exemplos seguintes mostram como analisar as medidas, dispositivos e os modelos de dispositivo ficheiros AVRO. Os exemplos correspondem aos exemplos descritos na secção anterior.

### <a name="read-avro-files-by-using-python"></a>Ler ficheiros AVRO com Python

#### <a name="install-pandas-and-the-pandavro-package"></a>Instalar o pacote de pandavro e o pandas

```python
pip install pandas
pip install pandavro
```

#### <a name="parse-a-measurements-avro-file"></a>Analisar um arquivo AVRO medidas

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
    # where each record is a single row.
    measurements = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The SystemProperties column contains a dictionary
    # with the device ID located under the connectionDeviceId key.
    transformed["device_id"] = measurements["SystemProperties"].apply(lambda x: x["connectionDeviceId"])

    # The Body column is a series of UTF-8 bytes that is stringified
    # and parsed as JSON. This example pulls the humidity property
    # from each column to get the humidity field.
    transformed["humidity"] = measurements["Body"].apply(lambda x: json.loads(bytes(x).decode('utf-8'))["humidity"])

    # Finally, print the new DataFrame with our device IDs and humidities.
    print(transformed)

```

#### <a name="parse-a-devices-avro-file"></a>Analisar um arquivo AVRO de dispositivos

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
    # where each record is a single row.
    devices = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device ID is available in the id column.
    transformed["device_id"] = devices["id"]

    # The template ID and version are present in a dictionary under
    # the deviceTemplate column.
    transformed["template_id"] = devices["deviceTemplate"].apply(lambda x: x["id"])
    transformed["template_version"] = devices["deviceTemplate"].apply(lambda x: x["version"])

    # The fanSpeed setting value is located in a nested dictionary
    # under the settings column.
    transformed["fan_speed"] = devices["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, print the new DataFrame with our device and template
    # information, along with the value of the fan speed.
    print(transformed)

```

#### <a name="parse-a-device-templates-avro-file"></a>Analisar um arquivo AVRO de modelos de dispositivo

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
    # where each record is a single row.
    templates = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The template and version are available in the id and version columns.
    transformed["template_id"] = templates["id"]
    transformed["template_version"] = templates["version"]

    # The fanSpeed setting value is located in a nested dictionary
    # under the settings column.
    transformed["fan_speed"] = templates["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, print the new DataFrame with our device and template
    # information, along with the value of the fan speed.
    print(transformed)
```

### <a name="read-avro-files-by-using-c"></a>Ficheiros do AVRO de leitura com o c#

#### <a name="install-the-microsofthadoopavro-package"></a>Instalar o pacote de Microsoft.Hadoop.Avro

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

#### <a name="parse-a-measurements-avro-file"></a>Analisar um arquivo AVRO medidas

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
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
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

#### <a name="parse-a-devices-avro-file"></a>Analisar um arquivo AVRO de dispositivos

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // records and make the function IEnumerable<AvroRecord>.
                    var deviceId = record.GetField<string>("id");

                    // The device template information is stored in a sub-record
                    // under the deviceTemplate field.
                    var deviceTemplateRecord = record.GetField<AvroRecord>("deviceTemplate");
                    var templateId = deviceTemplateRecord.GetField<string>("id");
                    var templateVersion = deviceTemplateRecord.GetField<string>("version");

                    // The settings and properties are nested two levels deep.
                    // The first level indicates settings or properties.
                    // The second level indicates the type of setting or property.
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

#### <a name="parse-a-device-templates-avro-file"></a>Analisar um arquivo AVRO de modelos de dispositivo

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // records and make the function IEnumerable<AvroRecord>.
                    var id = record.GetField<string>("id");
                    var version = record.GetField<string>("version");

                    // The settings and properties are nested two levels deep.
                    // The first level indicates settings or properties.
                    // The second level indicates the type of setting or property.
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

### <a name="read-avro-files-by-using-javascript"></a>Ler ficheiros AVRO com Javascript

#### <a name="install-the-avsc-package"></a>Instalar o pacote de avsc

```javascript
npm install avsc
```

#### <a name="parse-a-measurements-avro-file"></a>Analisar um arquivo AVRO medidas

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device ID and humidity from each record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device ID from the system properties.
        const deviceId = record.SystemProperties.connectionDeviceId;

        // Convert the body from a buffer to a string and parse it.
        const body = JSON.parse(record.Body.toString());

        // Get the humidty property from the body.
        const humidity = body.humidity;

        // Log the retrieved device ID and humidity.
        console.log(`Device ID: ${deviceId}`);
        console.log(`Humidity: ${humidity}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

#### <a name="parse-a-devices-avro-file"></a>Analisar um arquivo AVRO de dispositivos

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device and template identification
// information and the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device ID from the id property.
        const deviceId = record.id;

        // Fetch the template ID and version from the deviceTemplate property.
        const deviceTemplateId = record.deviceTemplate.id;
        const deviceTemplateVersion = record.deviceTemplate.version;

        // Get the fanSpeed from the nested device settings property.
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the retrieved device ID and humidity.
        console.log(`ID: ${deviceId}, Template ID: ${deviceTemplateId}, Template Version: ${deviceTemplateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

#### <a name="parse-a-device-templates-avro-file"></a>Analisar um arquivo AVRO de modelos de dispositivo

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device and template identification
// information and the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the template ID and version from the id and verison properties.
        const templateId = record.id;
        const templateVersion = record.version;

        // Get the fanSpeed from the nested device settings property.
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the retrieved device id and humidity.
        console.log(`Template ID: ${templateId}, Template Version: ${templateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como exportar os seus dados, avance para o passo seguinte:

> [!div class="nextstepaction"]
> [Como visualizar os seus dados no Power BI](howto-connect-powerbi.md)
