---
title: Utilizar dispositivos Azure IoT Edge como gateways | Documentos da Microsoft
description: Utilize o Azure IoT Edge para criar um transparente opaco ou dispositivo de gateway de proxy que envia dados de vários dispositivos de downstream para a cloud ou processa-os localmente.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: e1825bcdd8dbb06ef027919416b2d0532a7df9c2
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47422835"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>Como um dispositivo IoT Edge pode ser utilizado como um gateway

A finalidade de gateways em soluções de IoT é específica para a solução e combinar a conectividade do dispositivo com análises avançadas. O Azure IoT Edge pode ser utilizado para satisfazer as necessidades de todos os para um gateway de IoT, independentemente se estão relacionadas com a conectividade, a identidade ou análises avançadas. Padrões de gateway neste artigo só fazer referência às características de conectividade do dispositivo downstream e identidade de dispositivo, não como dados de dispositivo são processados no gateway.

## <a name="patterns"></a>Padrões
Existem três padrões para a utilização de um dispositivo IoT Edge como gateway: protocolo transparente, tradução e tradução de identidade:
* **Transparente** – dispositivos que, teoricamente, é podem ligar ao IoT Hub podem ligar a um dispositivo de gateway em vez disso. Isso implica que os dispositivos de downstream tem suas próprias identidades do IoT Hub e estiver a utilizar qualquer um dos protocolos MQTT, AMQP ou HTTP. O gateway de limite a passar comunicações entre os dispositivos e IoT Hub. Os dispositivos não estão cientes de que eles estão a comunicar com a cloud através de um gateway e um usuário interagir com os dispositivos no IoT Hub não está ciente do dispositivo de gateway intermédio. Portanto, o gateway é transparente. Consulte a [criar um gateway transparente] [ lnk-iot-edge-as-transparent-gateway] procedimentos para obter informações específicas sobre como utilizar um dispositivo IoT Edge como gateway transparente.
* **Tradução de protocolos** – também conhecido como padrão de gateway opaco, dispositivos que não suportam MQTT, AMQP ou HTTP utilização um dispositivo de gateway para enviar dados para o IoT Hub. O gateway é inteligente o suficiente para compreender esse protocolo utilizado pelos dispositivos downstream; No entanto é o único dispositivo que tenha a identidade do IoT Hub. Parece todas as informações que ele é proveniente de um dispositivo, o gateway. Isso implica que dispositivos downstream tem incorporar informações de identificação adicionais em suas mensagens se quiser que aplicações na cloud de ponderar sobre os dados numa base por dispositivo. Além disso, primitivos de IoT Hub como duplo e métodos só estão disponíveis para o dispositivo de gateway, os dispositivos não downstream.
* **Tradução de identidade** -os dispositivos que não é possível ligar ao IoT Hub ligar a um dispositivo de gateway que fornece o IoT Hub tradução de identidade e o protocolo em nome dos dispositivos de downstream. O gateway é inteligente o suficiente para compreender o protocolo utilizado pelos dispositivos downstream, identidade de fornecê-los e traduzir primitivos de IoT Hub. Dispositivos Downstream aparecem no IoT Hub como primeira classe dispositivos duplos e métodos. Um utilizador pode interagir com os dispositivos no IoT Hub e não reconhece o dispositivo de gateway intermédio.

![Diagramas de padrões de gateway][1]

## <a name="use-cases"></a>Casos de utilização
Todos os padrões de gateway fornecem as seguintes vantagens:
* **Análise de borda** – total de serviços de ia de uso localmente para processar dados provenientes de dispositivos de downstream sem enviar telemetria de fidelidade para a cloud. Encontre e reagir às informações localmente e enviar apenas um subconjunto de dados para o IoT Hub. 
* **Isolamento de dispositivo Downstream** – o dispositivo de gateway pode proteger todos os dispositivos de downstream da exposição à internet. Ele pode ser usada entre uma rede ou que não tem conectividade e uma rede de TI que fornece acesso à web. 
* **Ligação multiplexação** - todos os dispositivos para ligar ao IoT Hub através de um IoT Edge dispositivo irá utilizar a mesma ligação subjacente.
* **Suavização de tráfego** -dispositivo do IoT Edge irá automaticamente implementar um término exponencial em caso de limitação, ao persistir as mensagens localmente o IoT Hub. Isso fará com que sua solução resiliente para picos de tráfego.
* **Suporte offline limitado** - o dispositivo de gateway irá armazenar localmente as mensagens e duplo de atualizações que não pode ser entregue ao IoT Hub.

Também pode realizar uma tradução de protocolos do gateway faz análises avançadas, isolamento de dispositivo, a suavização de tráfego e suporte offline para dispositivos existentes e novos dispositivos que estão a restrição de recursos. Número de dispositivos existente está produzindo dados que podem potenciar as informações de negócio No entanto não foram projetados tendo em mente, a conectividade de cloud. Gateways opacos permitem estes dados para ser desbloqueada e utilizados numa solução de IoT ponto a ponto.

Um gateway que faz a tradução de identidade oferecem os benefícios de tradução de protocolos e adicionalmente permitir a capacidade de gerenciamento completa de downstream dispositivos a partir da cloud. Todos os dispositivos no seu IoT solução são apresentados no IoT Hub, independentemente do protocolo com eles falam.

## <a name="cheat-sheet"></a>Truques e dicas
Aqui está uma rápida e dicas que compara os primitivos de IoT Hub ao utilizar transparente, opaco (protocol) e do proxy de gateways.

| &nbsp; | Gateway transparente | Tradução de protocolos | Tradução de identidade |
|--------|-------------|--------|--------|
| Identidades armazenadas no registo de identidade do IoT Hub | Identidades de todos os dispositivos ligados | Apenas a identidade do dispositivo de gateway | Identidades de todos os dispositivos ligados |
| Dispositivo duplo | Cada dispositivo ligado tem seu próprio dispositivo duplo | Apenas o gateway tem um duplos de módulo e dispositivo | Cada dispositivo ligado tem seu próprio dispositivo duplo |
| Os métodos diretos e mensagens da cloud para dispositivo | A cloud pode resolver individualmente cada dispositivo ligado | Cloud apenas os pode resolver o dispositivo de gateway | A cloud pode resolver individualmente cada dispositivo ligado |
| [Quotas e limitações do IoT Hub][lnk-iothub-throttles-quotas] | Aplicar a cada dispositivo | Aplicam-se para o dispositivo de gateway | Aplicar a cada dispositivo |

Ao usar um padrão de gateway opaco (tradução do protocolo), todos os dispositivos que se conectam através desse gateway partilham a mesma fila de cloud para o dispositivo, que pode conter no máximo 50 mensagens. Ela segue o padrão gateway opaco deve ser utilizado apenas quando muito poucos dispositivos se ligarem através de cada gateway de campo e o tráfego de nuvem para o dispositivo é baixo.

## <a name="next-steps"></a>Passos Seguintes
Utilizar um dispositivo IoT Edge como um [gateway transparente][lnk-iot-edge-as-transparent-gateway] 

[lnk-iot-edge-as-transparent-gateway]: ./how-to-create-transparent-gateway-linux.md
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md

[1]: ./media/iot-edge-as-gateway/edge-as-gateway.png
