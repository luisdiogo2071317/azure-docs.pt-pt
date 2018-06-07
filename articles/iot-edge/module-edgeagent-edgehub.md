---
title: Referência do Azure IoT EdgeAgent e EdgeHub | Microsoft Docs
description: Reveja as propriedades específicas e os respetivos valores duplos de módulo edgeAgent e edgeHub
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/14/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0b9e7421bb09e619b4a820910db5faa9edfcc5d5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632912"
---
# <a name="properties-of-the-edge-agent-and-edge-hub-module-twins"></a>Propriedades do agente de limite e duplos de módulo de hub de limite

O agente de limite e o hub de limite são dois módulos que constituem o tempo de execução do limite de IoT. Para obter mais informações sobre os deveres cada módulo realiza, consulte [compreender o tempo de execução do limite do Azure IoT e respetiva arquitetura](iot-edge-runtime.md). 

Este artigo fornece as propriedades pretendidas e reportadas propriedades dos duplos do módulo de tempo de execução. Consulte [implementação e monitorização] [ lnk-deploy] para obter mais informações sobre como implementar módulos em dispositivos de limite de IoT.

## <a name="edgeagent-desired-properties"></a>Propriedades de EdgeAgent pretendida

O duplo de módulo para o agente de limite é chamado `$edgeAgent` e coordena as comunicações entre o agente de limite, em execução num dispositivo e do IoT Hub. As propriedades pretendidas são definidas ao aplicar um manifesto de implementação num dispositivo específico, como parte de uma implementação de único dispositivo ou à escala. 

| Propriedade | Descrição | Necessário |
| -------- | ----------- | -------- |
| schemaVersion | Tem de ser "1.0" | Sim |
| Runtime.Type | Tem de ser "docker" | Sim |
| runtime.settings.minDockerVersion | Definido para a versão mínima do Docker necessária para o manifesto de implementação | Sim |
| runtime.settings.loggingOptions | Um stringified JSON que contém as opções de registo para o contentor de agente de limite. [Opções de registo do docker][lnk-docker-logging-options] | Não |
| systemModules.edgeAgent.type | Tem de ser "docker" | Sim |
| systemModules.edgeAgent.settings.image | O URI da imagem do agente do limite. Atualmente, o agente de limite não é possível se Atualize automaticamente. | Sim |
| systemModules.edgeAgent.settings.createOptions | Um stringified JSON que contém as opções para a criação do contentor de agente Edge. [Docker criar opções][lnk-docker-create-options] | Não |
| systemModules.edgeAgent.configuration.id | O ID da implementação que implementado Este módulo. | Isto é definido pelo IoT Hub quando este manifesto é aplicado através de uma implementação. Não faz parte de um manifesto de implementação. |
| systemModules.edgeHub.type | Tem de ser "docker" | Sim |
| systemModules.edgeHub.status | Tem de ser "em execução" | Sim |
| systemModules.edgeHub.restartPolicy | Tem de ser "sempre" | Sim |
| systemModules.edgeHub.settings.image | O URI da imagem do Edge hub. | Sim |
| systemModules.edgeHub.settings.createOptions | Um stringified JSON que contém as opções para a criação do contentor de hub de limite. [Docker criar opções][lnk-docker-create-options] | Não |
| systemModules.edgeHub.configuration.id | O ID da implementação que implementado Este módulo. | Isto é definido pelo IoT Hub quando este manifesto é aplicado através de uma implementação. Não faz parte de um manifesto de implementação. |
| módulos. .version {moduleId} | Uma cadeia definida pelo utilizador, que representa a versão deste módulo. | Sim |
| módulos. .type {moduleId} | Tem de ser "docker" | Sim |
| modules.{moduleId}.restartPolicy | {"nunca" \| "no-falhou" \| "no-mau estado de funcionamento" \| "sempre"} | Sim |
| modules.{moduleId}.settings.image | O URI para a imagem do módulo. | Sim |
| modules.{moduleId}.settings.createOptions | Um stringified JSON que contém as opções para a criação do contentor do módulo. [Docker criar opções][lnk-docker-create-options] | Não |
| modules.{moduleId}.configuration.id | O ID da implementação que implementado Este módulo. | Isto é definido pelo IoT Hub quando este manifesto é aplicado através de uma implementação. Não faz parte de um manifesto de implementação. |

## <a name="edgeagent-reported-properties"></a>EdgeAgent comunicadas propriedades

O agente de limite reportados propriedades incluem as três principais informações:

1. O estado da aplicação das propriedades pretendidas vistos por último;
2. O estado dos módulos atualmente em execução no dispositivo, conforme comunicado pelo agente de limite; e
3. Uma cópia das propriedades pretendidas atualmente em execução no dispositivo.

Neste último elemento de informação é útil caso as propriedades pretendidas mais recente não são aplicadas com êxito pelo runtime e o dispositivo ainda está em execução um manifesto de implementação anterior.

> [!NOTE]
> As propriedades do agente Edge comunicadas são úteis como podem ser consultadas com o [idioma de consulta do IoT Hub] [ lnk-iothub-query] para investigar o estado de implementações de escala. Consulte [implementações] [ lnk-deploy] para obter mais informações sobre como utilizar esta funcionalidade.

A tabela seguinte não inclui as informações que são copiadas a partir das propriedades pretendidas.

| Propriedade | Descrição |
| -------- | ----------- |
| lastDesiredVersion | Este número inteiro refere-se para a última versão das propriedades pretendidas processados pelo agente de limite. |
| lastDesiredStatus.code | Este é o código de estado que faça referência a última propriedades pretendidas vistas pelo agente de limite. Valores permitidos: `200` com êxito, `400` configuração inválida, `412` versão de esquema inválida, `417` as propriedades pretendidas estão vazios, `500` falhou |
| lastDesiredStatus.description | Descrição de texto do Estado |
| DeviceHealth | `healthy` Se o estado do tempo de execução de todos os módulos `running` ou `stopped`, `unhealthy` caso contrário |
| configurationHealth.{deploymentId}.health | `healthy` Se o estado do tempo de execução de todos os módulos definido pela implementação {deploymentId} `running` ou `stopped`, `unhealthy` caso contrário |
| runtime.platform.OS | Relatórios de SO em execução no dispositivo |
| runtime.platform.architecture | A arquitetura da CPU de relatórios no dispositivo |
| systemModules.edgeAgent.runtimeStatus | O estado do agente de limite: {"em execução" \| "mau estado de funcionamento"} |
| systemModules.edgeAgent.statusDescription | Descrição de texto do Estado do agente Edge comunicado. |
| systemModules.edgeHub.runtimeStatus | Estado atual do hub de limite: {"em execução" \| "parado" \| "Falha" \| "término" \| "mau estado de funcionamento"} |
| systemModules.edgeHub.statusDescription | Descrição de texto do estado atual do hub de limite se mau estado de funcionamento. |
| systemModules.edgeHub.exitCode | Se terminado, o código de saída comunicado pelo contentor de hub de limite |
| systemModules.edgeHub.startTimeUtc | Tempo quando Edge hub foi iniciado pela última vez |
| systemModules.edgeHub.lastExitTimeUtc | Quando terminado pela última vez hub de limite de tempo |
| systemModules.edgeHub.lastRestartTimeUtc | Tempo quando Edge hub foi reiniciado pela última vez |
| systemModules.edgeHub.restartCount | Número de vezes que este módulo foi reiniciado como parte da política de reinício. |
| modules.{moduleId}.runtimeStatus | Estado atual do módulo: {"em execução" \| "parado" \| "Falha" \| "término" \| "mau estado de funcionamento"} |
| modules.{moduleId}.statusDescription | Descrição de texto do estado atual do módulo se mau estado de funcionamento. |
| modules.{moduleId}.exitCode | Se terminado, o código de saída comunicado pelo contentor de módulo |
| modules.{moduleId}.startTimeUtc | Tempo quando o módulo foi iniciado pela última vez |
| modules.{moduleId}.lastExitTimeUtc | Tempo quando o módulo terminado pela última vez |
| modules.{moduleId}.lastRestartTimeUtc | Tempo quando o módulo foi reiniciado pela última vez |
| modules.{moduleId}.restartCount | Número de vezes que este módulo foi reiniciado como parte da política de reinício. |

## <a name="edgehub-desired-properties"></a>Propriedades de EdgeHub pretendida

O duplo de módulo para o hub de limite é chamado `$edgeHub` e coordena as comunicações entre o hub de limite que executem um dispositivo e o IoT Hub. As propriedades pretendidas são definidas ao aplicar um manifesto de implementação num dispositivo específico, como parte de uma implementação de único dispositivo ou à escala. 

| Propriedade | Descrição | Necessário no manifesto de implementação |
| -------- | ----------- | -------- |
| schemaVersion | Tem de ser "1.0" | Sim |
| routes.{routeName} | Uma cadeia que representa uma rota de hub de limite. | O `routes` elemento pode estar presente, mas vazio. |
| storeAndForwardConfiguration.timeToLiveSecs | O tempo em segundos, que mantém o hub de limite mensagens no caso de desligado encaminhamento pontos finais, por exemplo, desligadas do IoT Hub ou módulo local | Sim |

## <a name="edgehub-reported-properties"></a>EdgeHub comunicadas propriedades

| Propriedade | Descrição |
| -------- | ----------- |
| lastDesiredVersion | Este número inteiro refere-se para a última versão das propriedades pretendidas processados através do hub de limite. |
| lastDesiredStatus.code | Este é o código de estado que faça referência a última propriedades pretendidas vistas pelo Edge hub. Valores permitidos: `200` com êxito, `400` configuração inválida, `500` falhou |
| lastDesiredStatus.description | Descrição de texto do Estado |
| clientes. .status {identidade de dispositivo ou módulo} | O estado de conectividade deste dispositivo ou o módulo. Os valores possíveis {"ligada" \| "desligada"}. Apenas as identidades do módulo podem estar no estado desligado. Dispositivos a jusante estabelecer ligação ao Edge hub aparecem apenas quando estiver ligado. |
| clientes. .lastConnectTime {identidade de dispositivo ou módulo} | Última hora no dispositivo ou módulo ligado |
| clientes. .lastDisconnectTime {identidade de dispositivo ou módulo} | Última vez que o dispositivo ou módulo desligado |

## <a name="next-steps"></a>Passos Seguintes

Para saber como utilizar estas propriedades para criar os manifestos de implementação, consulte [compreender como os módulos de IoT limite podem ser utilizados, configurados e reutilizados](module-composition.md).

<!--links -->
[lnk-deploy]: module-deployment-monitoring.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
