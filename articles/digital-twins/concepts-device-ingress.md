---
title: Entrada de conectividade e telemetria de dispositivos duplos Digital do Azure | Documentos da Microsoft
description: Descrição geral da integração de um dispositivo duplos Digital do Azure
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: 7eddea7e0d57b89318232da6f086bbe2f649ee77
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50211932"
---
# <a name="device-connectivity-and-telemetry-ingress"></a>Entrada de conectividade e telemetria de dispositivo

Os dados de telemetria enviados pelos dispositivos e sensores formam a estrutura principal de qualquer solução de IoT. Como tal, que representa estes recursos díspares e gerenciá-las dentro do contexto de uma localização são uma preocupação chefe no desenvolvimento de aplicações de IoT. Duplos Digital do Azure simplifica a desenvolver soluções de IoT, unindo dispositivos e sensores com um gráfico de inteligência espaciais.

Para começar, um recurso do IoT Hub deve ser criado na raiz do gráfico espacial, permitindo que todos os dispositivos abaixo o espaço de raiz para enviar mensagens. Assim que o IoT Hub tiver sido criado, e dispositivos com sensores foram registrados dentro da instância de duplos Digital, os dispositivos podem começar a enviar dados para um serviço de duplos Digital através da [SDK de dispositivo do IoT do Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks#azure-iot-device-sdks).

Um guia passo a passo para dispositivos de introdução pode ser encontrado no [Tutorial para implementar e configurar duplos Digital](tutorial-facilities-setup.md). Visão geral, as etapas são:

- Implementar uma instância de duplos Digital do Azure a partir do [portal do Azure](https://portal.azure.com)
- Criar espaços no seu gráfico
- Criar um recurso do IoT Hub e atribuí-lo a um espaço no seu gráfico
- Crie dispositivos e sensores no seu gráfico e atribuí-las para os espaços criados nos passos acima
- Criar na ferramenta para filtrar mensagens de telemetria com base nas condições de correspondência
- Criar uma [função definida pelo utilizador](concepts-user-defined-functions.md) e atribuí-lo a um espaço no gráfico para o processamento personalizado de suas mensagens de telemetria
- Atribuir uma função para permitir que a função definida pelo utilizador acessar os dados de gráfico
- Obter a cadeia de ligação de dispositivos do IoT Hub a partir das APIs de gestão de duplos Digital
- Configurar a cadeia de ligação do dispositivo no dispositivo com o SDK de dispositivo do IoT do Azure

Veja a seguir aprenderá como obter a cadeia de ligação de dispositivos do IoT Hub a partir da API de gestão de duplos digitais e como utilizar o formato de mensagem de telemetria do IoT Hub para enviar telemetria com base em sensor. Duplos digitais requer que cada parte de telemetria que recebe a ser associado um sensor dentro do gráfico para garantir que os dados geográfico é processada e encaminhada dentro do contexto geográfico apropriado.

## <a name="get-the-iot-hub-device-connection-string-from-the-management-api"></a>Obter a cadeia de ligação de dispositivos do IoT Hub a partir da API de gestão

Fazer uma chamada GET na API do dispositivo com um `includes=ConnectionString` parâmetro para obter a cadeia de ligação de dispositivos do IoT Hub. Pode filtrar por ID de GUID ou do hardware do dispositivo para localizar o dispositivo especificado:

```plaintext
https://yourManagementApiUrl/api/v1.0/devices/yourDeviceGuid?includes=ConnectionString
```

```plaintext
https://yourManagementApiUrl/api/v1.0/devices?hardwareIds=yourDeviceHardwareId&includes=ConnectionString
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| *yourManagementApiUrl* | O caminho do URL completo para a API de gestão |
| *yourDeviceGuid* | O ID de dispositivo |
| *yourDeviceHardwareId* | O ID de hardware do dispositivo |

No payload de resposta, copiar do dispositivo *connectionString* propriedade, que irá utilizar ao chamar o SDK de dispositivo do IoT do Azure para enviar dados para Twins Digital do Azure.

## <a name="device-to-cloud-message"></a>Dispositivo a mensagem de cloud

Pode personalizar o formato de mensagem do seu dispositivo e payload de acordo com as necessidades da sua solução. Pode usar qualquer contrato de dados que pode ser serializado numa matriz de bytes ou fluxo que é suportado pelos [classe de mensagem de cliente de dispositivo de IoT do Azure, mensagem (byte [] byteArray)](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.-ctor?view=azure-dotnet#Microsoft_Azure_Devices_Client_Message__ctor_System_Byte___). A mensagem pode ser um formato binário personalizado à sua escolha, desde que decodificar o contrato de dados numa função definida pelo usuário correspondente. O único requisito para uma mensagem de dispositivo-para-Cloud é manter um conjunto de propriedades (ver abaixo) para garantir a que sua mensagem é devidamente encaminhada para o motor de processamento.

### <a name="telemetry-properties"></a>Propriedades de telemetria

Enquanto o conteúdo do payload de um **mensagem** podem ser dados arbitrários cópia de segurança para 256 kb de tamanho, existem alguns requisitos no esperado [ `Message.Properties` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.properties?view=azure-dotnet). Os passos descritos abaixo refletem as propriedades necessárias e opcionais são suportadas pelo sistema:

| Nome da propriedade | Valor | Necessário | Descrição |
|---|---|---|---|
| *DigitalTwins telemetria* | 1.0 | Sim | Um valor constante que identifica uma mensagem para o sistema |
| *DigitalTwins SensorHardwareId* | `string(72)` | Sim | Um identificador exclusivo do envio de sensor a **mensagem**. Este valor tem de corresponder um objeto **HardwareId** propriedade para o sistema para processá-lo. Por exemplo, `00FF0643BE88-CO2` |
| *CreationTimeUtc* | `string` | Não | Uma [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) formatado identificar o tempo de amostragem da carga de cadeia de data. Por exemplo, `2018-09-20T07:35:00.8587882-07:00` |
| *correlationId* | `string` | Não | UUID que pode ser utilizado para eventos de rastreio em todo o sistema. Por exemplo, `cec16751-ab27-405d-8fe6-c68e1412ce1f`

### <a name="sending-your-message-to-digital-twins"></a>Enviar a mensagem a duplos Digital

Utilizar o DeviceClient [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventasync?view=azure-dotnet) ou [SendEventBatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventbatchasync?view=azure-dotnet) chamada para enviar a mensagem ao seu serviço de duplos Digital.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o processamento de dados e as capacidades de funções definidas pelo utilizador dos gémeos Digital do Azure, leia [processamento de dados de duplos Digital do Azure e funções definidas pelo utilizador](concepts-user-defined-functions.md).

