---
title: Metadados de informações do dispositivo na solução de monitorização remota | Documentos da Microsoft
description: Uma descrição sobre a solução pré-configurada de monitorização remota e respetiva arquitetura do Azure IoT.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 1b334769-103b-4eb0-a293-184f3d1ba9a3
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 4efea316c05f566add3e175bc5bb18842225ede3
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35758166"
---
# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Metadados de informações do dispositivo na solução pré-configurada de monitorização remota

A solução pré-configurada de monitorização remota do Azure IoT Suite demonstra uma abordagem para o gerenciamento de metadados do dispositivo. Este artigo descreve a abordagem que esta solução necessário para que possa compreender:

* Os metadados do dispositivo em que a solução armazena.
* Como a solução gerencia os metadados do dispositivo.

## <a name="context"></a>Contexto

A monitorização remota pré-configurada a solução utiliza [IoT Hub do Azure] [ lnk-iot-hub] para permitir que os seus dispositivos enviar dados para a cloud. A solução armazena informações sobre os dispositivos nas três localizações diferentes:

| Localização | Informações armazenadas | Implementação |
| -------- | ------------------ | -------------- |
| Registo de identidade | Estado ativado do id de dispositivo, as chaves de autenticação | Integrado ao IoT Hub |
| Dispositivos duplos | Metadados: propriedades comunicadas, as propriedades pretendidas, etiquetas | Integrado ao IoT Hub |
| BD do Cosmos | Histórico de comando e o método | Personalizado para a solução |

IoT Hub inclui um [registo de identidade do dispositivo] [ lnk-identity-registry] para gerir o acesso para um hub IoT e utiliza [dispositivos duplos] [ lnk-device-twin] para gerir os metadados do dispositivo. Também há um remoto monitorização específicos da solução *registo de dispositivo* que armazena o histórico de comando e de método. Solução de monitorização remota utiliza uma [Cosmos DB] [ lnk-docdb] base de dados para implementar um arquivo personalizado para o histórico de comando e de método.

> [!NOTE]
> A solução pré-configurada de monitorização remota mantém o registo de identidade de dispositivo em sincronia com as informações na base de dados do Cosmos DB. Ambos utilizam o mesmo id de dispositivo para identificar exclusivamente cada dispositivo ligado ao seu hub IoT.

## <a name="device-metadata"></a>Metadados do dispositivo

O IoT Hub mantém um [dispositivo duplo] [ lnk-device-twin] para cada dispositivo simulado e físico ligado a uma solução de monitorização remota. A solução utiliza dispositivos duplos para gerir os metadados associados a dispositivos. Um dispositivo duplo é um documento JSON mantido pelo IoT Hub e a solução utiliza a API do Hub IoT para interagir com dispositivos duplos.

Um dispositivo duplo armazena três tipos de metadados:

- *Propriedades comunicadas* são enviadas para um hub IoT por um dispositivo. Na solução de monitorização remota, dispositivos simulados enviam as propriedades comunicadas no arranque e em resposta às **alterar estado do dispositivo** comandos e métodos. Pode ver as propriedades comunicadas no **lista de dispositivos** e **detalhes do dispositivo** no portal da solução. Propriedades comunicadas são só de leitura.
- *Propriedades pretendidas* são obtidos a partir do IoT hub pelos dispositivos. É da responsabilidade do dispositivo para fazer qualquer alteração no dispositivo de configuração necessária. Também é responsabilidade do dispositivo para relatar a alteração para o hub como uma propriedade comunicada. Pode definir um valor de propriedade pretendida no portal de solução.
- *Etiquetas* só existem no dispositivo duplo e nunca são sincronizadas com um dispositivo. Pode definir valores de etiqueta no portal da solução e usá-las ao filtrar a lista de dispositivos. A solução também utiliza uma etiqueta para identificar o ícone para apresentar para um dispositivo no portal da solução.

Exemplo comunicado propriedades dos dispositivos simulados incluem fabricante, o número de modelo, latitude e longitude. Dispositivos simulados também devolvem a lista de métodos com suporte como uma propriedade comunicada.

> [!NOTE]
> O código do dispositivo simulado utiliza apenas as propriedades pretendidas **Desired.Config.TemperatureMeanValue** e **Desired.Config.TelemetryInterval** para atualizar as propriedades comunicadas enviadas de volta para o Hub IoT. Todos os outros pedidos de alteração de propriedades pretendidas são ignorados.

Um documento de metadados de informações do dispositivo JSON armazenado na base de dados do Gestor de registro de dispositivo do Cosmos DB tem a seguinte estrutura:

```json
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

> [!NOTE]
> Informações do dispositivo também podem incluir metadados para descrever a telemetria que o dispositivo envia para o IoT Hub. Solução de monitorização remota utiliza estes metadados de telemetria para personalizar a forma como o dashboard é apresentado [a telemetria dinâmica][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Ciclo de vida

Quando cria um dispositivo pela primeira vez no portal da solução, a solução cria uma entrada na base de dados do Cosmos DB para armazenar o histórico de comando e de método. Neste momento, a solução também cria uma entrada para o dispositivo no registo de identidade do dispositivo, que gera as chaves que o dispositivo utiliza para autenticar com o IoT Hub. Ele também cria um dispositivo duplo.

Quando um dispositivo se liga inicialmente a solução, envia as propriedades comunicadas e uma mensagem informativa do dispositivo. Os valores de propriedades comunicadas são guardados automaticamente no dispositivo duplo. As propriedades reportadas incluem o fabricante do dispositivo, número de modelo, número de série e uma lista de métodos suportados. A mensagem de informações de dispositivos inclui a lista de comandos que o dispositivo suporta incluindo informações sobre quaisquer parâmetros de comando. Quando a solução recebe esta mensagem, ela atualiza as informações do dispositivo na base de dados do Cosmos DB.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Ver e editar informações do dispositivo no portal da solução

A lista de dispositivos no portal de solução apresenta as seguintes propriedades do dispositivo como colunas por predefinição: **Status**, **DeviceId**, **fabricante**, **modelo Número**, **número de série**, **Firmware**, **plataforma**, **processador**, e  **Instalado RAM**. Pode personalizar as colunas ao clicar em **editor de colunas**. As propriedades do dispositivo **Latitude** e **Longitude** unidade a localização no mapa do Bing no dashboard.

![Editor de colunas na lista de dispositivos][img-device-list]

Na **detalhes do dispositivo** painel no portal da solução, pode editar as propriedades pretendidas e etiquetas (comunicado propriedades são só de leitura).

![Painel de detalhes do dispositivo][img-device-edit]

Pode utilizar o portal de solução para remover um dispositivo da solução. Quando remove um dispositivo, a solução remove a entrada de dispositivo do registo de identidade e, em seguida, elimina o dispositivo duplo. A solução também remove informações relacionadas com o dispositivo da base de dados do Cosmos DB. Antes de poder remover um dispositivo, terá de o desativar.

![Remover o dispositivo][img-device-remove]

## <a name="device-information-message-processing"></a>Processamento de mensagens de informações do dispositivo

As mensagens de informações de dispositivo enviadas por um dispositivo são diferentes de mensagens de telemetria. Mensagens de informações do dispositivo incluem os comandos de que um dispositivo pode responder e qualquer histórico de comando. O IoT Hub em si não tem conhecimento de metadados contidos numa mensagem informativa do dispositivo e processar a mensagem da mesma forma que processa qualquer mensagem de dispositivo para a cloud. Na solução de monitorização remota, um [do Azure Stream Analytics] [ lnk-stream-analytics] tarefa (ASA) lê as mensagens do IoT Hub. O **DeviceInfo** filtros para mensagens que contêm da tarefa do stream analytics **"ObjectType": "DeviceInfo"** e os reencaminha para o **EventProcessorHost** instância de host que é executada num trabalho web. Lógica da **EventProcessorHost** instância utiliza o ID de dispositivo para encontrar o registo do Cosmos DB para o dispositivo específico e atualizar o registo.

> [!NOTE]
> Uma mensagem de informações do dispositivo é uma mensagem padrão do dispositivo para a cloud. A solução distingue entre mensagens de telemetria e mensagens de informações do dispositivo através de consultas do ASA.

## <a name="next-steps"></a>Passos Seguintes

Agora que concluiu a aprendizagem de como pode personalizar soluções pré-configuradas, pode explorar algumas das outras funcionalidades e capacidades das soluções pré-configuradas do IoT Suite:

* [Descrição de geral da solução pré-configurada de manutenção preditiva][lnk-predictive-overview]
* [Perguntas mais frequentes sobre o IoT Suite][lnk-faq]
* [Segurança de IoT desde o início][lnk-security-groundup]

<!-- Images and links -->
[img-device-list]: media/iot-suite-v1-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-v1-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-v1-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dynamic-telemetry]: iot-suite-v1-dynamic-telemetry.md

[lnk-predictive-overview]:../iot-accelerators/iot-accelerators-predictive-overview.md
[lnk-faq]: iot-suite-v1-faq.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up
