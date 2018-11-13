---
title: Desenvolver módulos do Azure IoT Edge | Documentos da Microsoft
description: Saiba como criar módulos personalizados para o Azure IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: cb97e2cf6d554753f64afc76de84f43e38443909
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567235"
---
# <a name="understand-the-requirements-and-tools-for-developing-iot-edge-modules"></a>Compreender os requisitos e as ferramentas para desenvolver módulos do IoT Edge

Este artigo explica o que funcionalidades estão disponíveis ao escrever aplicativos que são executados como módulo do IoT Edge e como tirar proveito delas.

## <a name="iot-edge-runtime-environment"></a>Ambiente de runtime do IoT Edge
O runtime do IoT Edge fornece a infraestrutura para integrar a funcionalidade de vários módulos do IoT Edge e implementá-las em dispositivos IoT Edge. Num alto nível, qualquer programa pode ser empacotado como um módulo do IoT Edge. No entanto, para tirar partido do IoT Edge, comunicação e funcionalidades de gestão, um programa em execução num módulo pode ligar-se para o hub IoT Edge local, integrado no runtime do IoT Edge.

## <a name="using-the-iot-edge-hub"></a>Utilizar o hub IoT Edge
O hub IoT Edge fornece duas funcionalidades principais: proxy para o IoT Hub e comunicações locais.

### <a name="iot-hub-primitives"></a>Primitivos de IoT Hub
IoT Hub vê uma instância de módulo analogously para um dispositivo, no sentido de que:

* tem um duplo do módulo que é distinto e isolado a partir do [dispositivo duplo](../iot-hub/iot-hub-devguide-device-twins.md) e os outros duplos de módulo de que o dispositivo;
* Pode enviar [mensagens dispositivo-para-cloud](../iot-hub/iot-hub-devguide-messaging.md);
* pode receber [métodos diretos](../iot-hub/iot-hub-devguide-direct-methods.md) direcionado especificamente para a sua identidade.

Atualmente, um módulo não é possível receber mensagens da cloud para dispositivo nem utilizar a funcionalidade de carregamento de ficheiros.

Ao escrever um módulo, pode utilizar o [SDK de dispositivo do IoT do Azure](../iot-hub/iot-hub-devguide-sdks.md) para se ligar ao hub IoT Edge e utilizar a funcionalidade acima, tal como faria ao utilizar o IoT Hub com um aplicativo de dispositivo, a única diferença é que, a partir da sua aplicação back-end, terá de fazer referência à identidade do módulo em vez da identidade de dispositivo.

Ver [desenvolver e implementar um módulo do IoT Edge num dispositivo simulado](tutorial-csharp-module.md) para obter um exemplo de um aplicativo de módulo que envia mensagens do dispositivo para a cloud e utiliza o módulo duplo.

### <a name="device-to-cloud-messages"></a>Mensagens do dispositivo para a cloud
Para ativar complexas de processamento de mensagens do dispositivo para a cloud, hub do IoT Edge fornece declarativa de encaminhamento de mensagens entre módulos e entre módulos e o IoT Hub. Encaminhamento declarativa permite que os módulos interceptar e processar mensagens enviadas por outros módulos e propagação-las em pipelines de complexos. O artigo [composição do módulo](module-composition.md) explica como compor módulos em pipelines complexos com rotas.

Módulo do IoT Edge, em vez de um aplicativo de dispositivo do IoT Hub normal, pode receber mensagens de dispositivo para a nuvem que estão a ser transmitidas por proxy ao seu hub IoT Edge local para processá-las.

Hub do IoT Edge propaga as mensagens ao módulo com base nas rotas declarativas descritas na [composição do módulo](module-composition.md) artigo. Ao desenvolver um módulo do IoT Edge, pode receber essas mensagens através da definição de manipuladores de mensagens.

Para simplificar a criação de rotas, IoT Edge adiciona o conceito de módulo *entrada* e *saída* pontos de extremidade. Um módulo pode receber todas as mensagens de dispositivo-para-cloud encaminhadas para o mesmo sem especificar qualquer entrada e pode enviar mensagens dispositivo-para a cloud sem especificar quaisquer dados.
No entanto, uso explícitas entradas e saídas, torna as regras de roteamento mais simples de entender. Para obter mais informações sobre regras de encaminhamento e pontos finais de entrada e saídos para módulos, consulte [composição do módulo](module-composition.md).

Por fim, as mensagens de dispositivo-para-cloud manipuladas pelo Edge hub são marcadas com as seguintes propriedades do sistema:

| Propriedade | Descrição |
| -------- | ----------- |
| $connectionDeviceId | O ID de dispositivo do cliente que enviou a mensagem |
| $connectionModuleId | O ID de módulo do módulo que enviou a mensagem |
| $inputName | A entrada que recebeu esta mensagem. Pode estar vazio. |
| $outputName | A saída utilizada para enviar a mensagem. Pode estar vazio. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Ligar ao hub IoT Edge, a partir de um módulo
Ligar ao hub IoT Edge local a partir de um módulo envolve dois passos: 
1. Utilize a cadeia de ligação fornecida pelo runtime do IoT Edge, quando seu módulo é iniciado.
2. Certifique-se de que seu aplicativo aceita o certificado apresentado pelo hub IoT Edge nesse dispositivo.

Cadeia de ligação a utilizar é injetada pelo runtime do IoT Edge na variável de ambiente `EdgeHubConnectionString`. Isto torna disponível para qualquer programa que queira utilizá-la.

Analogously, o certificado a utilizar para validar a ligação do hub IoT Edge é injetado pelo runtime do IoT Edge num arquivo cujo caminho está disponível na variável de ambiente `EdgeModuleCACertificateFile`.

## <a name="next-steps"></a>Passos Seguintes

Depois de desenvolver um módulo, saiba como [implementar e monitorizar os módulos do IoT Edge em escala](how-to-deploy-monitor.md).

