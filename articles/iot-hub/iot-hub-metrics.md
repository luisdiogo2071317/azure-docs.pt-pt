---
title: Utilizar métricas para monitorizar o IoT Hub do Azure | Documentos da Microsoft
description: Como utilizar as métricas do IoT Hub do Azure para avaliar e monitorizar o estado de funcionamento geral os hubs IoT.
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/25/2017
ms.author: nberdy
ms.openlocfilehash: b41458f0201c46b99c09d0bfffd219743a36ad50
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186760"
---
# <a name="understand-iot-hub-metrics"></a>Compreender as métricas do IoT Hub
IoT Hub métricas oferecem-lhe dados melhor sobre o estado dos recursos de IoT do Azure na sua subscrição do Azure. Métricas do IoT Hub permitem-lhe avaliar o estado de funcionamento geral do serviço IoT Hub e dispositivos conectados a ele. Destinada ao utilizador estatísticas são importantes porque eles ajudá-lo a ver o que está acontecendo com problemas de causa raiz seu IoT hub e ajuda sem a necessidade de contactar o suporte do Azure.

As métricas estão ativadas por predefinição. Pode ver as métricas do IoT Hub no portal do Azure.

## <a name="how-to-view-iot-hub-metrics"></a>Como ver as métricas do IoT Hub
1. Crie um hub IoT. Pode encontrar instruções sobre como criar um hub IoT a [começar] [ lnk-get-started] guia.
2. Abra o painel do IoT hub. A partir daí, clique em **métricas**.
   
    ![][1]
3. No painel métricas, pode ver as métricas para o seu hub IoT e criar vistas personalizadas, as métricas. Pode optar por enviar os seus dados de métricas para um ponto de extremidade de Hubs de eventos ou uma conta de armazenamento do Azure ao clicar em **as definições de diagnóstico**.
   
    ![][2]

## <a name="iot-hub-metrics-and-how-to-use-them"></a>Métricas do IoT Hub e como utilizá-los
O IoT Hub fornece várias métricas para uma visão geral do Estado de funcionamento do seu hub e o número total de dispositivos ligados. Pode combinar informações a partir de várias métricas para pintar uma visão mais ampla do Estado do IoT hub. A tabela seguinte descreve as métricas que controla cada hub IoT e como cada uma se relaciona com o estado geral do IoT hub.

|Métrica|Nome a apresentar de métrica|Unidade|Tipo de agregação|Descrição|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Tentativas de envio de mensagens de telemetria|Contagem|Total|Número de mensagens de telemetria do dispositivo-para-cloud tentada a serem enviados ao seu hub IoT|
|d2c.telemetry.ingress.success|Mensagens de telemetria enviadas|Contagem|Total|Número de mensagens de telemetria do dispositivo-para-cloud enviado com êxito ao seu hub IoT|
|c2d.commands.egress.complete.success|Comandos concluídos|Contagem|Total|Número de comandos do cloud-para-dispositivo foi concluída com êxito pelo dispositivo|
|c2d.commands.egress.abandon.success|Comandos abandonados|Contagem|Total|Número de comandos do cloud-para-dispositivo abandonadas pelo dispositivo|
|c2d.commands.egress.reject.success|Comandos rejeitados|Contagem|Total|Número de comandos do cloud-para-dispositivo rejeitado pelo dispositivo|
|devices.totalDevices|Total de dispositivos|Contagem|Total|Número de dispositivos registados no seu hub IoT|
|devices.connectedDevices.allProtocol|Dispositivos ligados|Contagem|Total|Número de dispositivos ligados ao seu hub IoT|
|d2c.telemetry.egress.success|Mensagens de telemetria entregues|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais (total)|
|d2c.telemetry.egress.dropped|Mensagens não processadas|Contagem|Total|Número de mensagens removida porque eles não correspondeu a quaisquer rotas e a rota de contingência foi desativada|
|d2c.telemetry.egress.orphaned|Mensagens órfãos|Contagem|Total|A contagem de mensagens não corresponde a quaisquer rotas, incluindo a rota de contingência|
|d2c.telemetry.egress.invalid|Mensagens inválidas|Contagem|Total|A contagem de mensagens não entregues devido a incompatibilidade com o ponto final|
|d2c.telemetry.egress.fallback|Mensagens que correspondem a condição de contingência|Contagem|Total|Número de mensagens escritas para o ponto final de contingência|
|d2c.endpoints.egress.eventHubs|Mensagens entregues aos pontos finais do Hub de eventos|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais do Hub de eventos|
|d2c.endpoints.latency.eventHubs|Latência de mensagem para pontos finais do Hub de eventos|Milissegundos|Média|A latência média entre a receção de mensagem para o hub IoT e entrada de mensagem num ponto de extremidade do Hub de eventos, em milissegundos|
|d2c.endpoints.egress.serviceBusQueues|Mensagens entregues aos pontos finais de fila do Service Bus|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais de fila do Service Bus|
|d2c.endpoints.latency.serviceBusQueues|Latência de mensagem para pontos finais de fila do Service Bus|Milissegundos|Média|A latência média entre a receção de mensagem para o hub IoT e entrada de mensagem num ponto final de fila do Service Bus, em milissegundos|
|d2c.endpoints.egress.serviceBusTopics|Mensagens entregues aos pontos finais do tópico do Service Bus|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais do tópico do Service Bus|
|d2c.endpoints.latency.serviceBusTopics|Latência de mensagem para pontos finais do tópico do Service Bus|Milissegundos|Média|A latência média entre a receção de mensagem para o hub IoT e entrada de mensagem num ponto de final do tópico do Service Bus, em milissegundos|
|d2c.endpoints.egress.builtIn.events|Mensagens entregues para o ponto final incorporado (mensagens/eventos)|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito para o ponto final incorporado (mensagens/eventos)|
|d2c.endpoints.latency.builtIn.events|Latência de mensagem para o ponto final incorporado (mensagens/eventos)|Milissegundos|Média|A latência média entre a receção de mensagem para o hub IoT e entrada de mensagem para o ponto final incorporado (mensagens/eventos), em milissegundos |
|d2c.twin.read.success|Lê a partir do dispositivos duplos com êxito|Contagem|Total|Contagem de leituras de iniciada pelo dispositivo duplo tudo com êxito.|
|d2c.twin.read.failure|Falha de leituras de twin de dispositivos|Contagem|Total|A contagem de todos os falha leituras iniciada pelo dispositivo duplo.|
|d2c.twin.read.size|Tamanho de resposta de leituras de twin de dispositivos|Bytes|Média|A média, Mín e máx. de tudo com êxito iniciada pelo dispositivo duplo leituras.|
|d2c.twin.update.success|Atualizações de duplo com êxito a partir de dispositivos|Contagem|Total|Contagem de todos os com êxito atualizações de iniciada pelo dispositivo duplo.|
|d2c.twin.update.failure|Falha de atualizações de twin de dispositivos|Contagem|Total|A contagem de todos os falha atualizações iniciadas pelo dispositivo duplo.|
|d2c.twin.update.size|Tamanho de atualizações de twin de dispositivos|Bytes|Média|A média, Mín e tamanho máximo de todos os bem-sucedida iniciada pelo dispositivo duplo atualizações.|
|c2d.methods.Success|Invocações de método direto com êxito|Contagem|Total|Contagem de chamadas de método direto tudo com êxito.|
|c2d.methods.Failure|Falha de invocações de método direto|Contagem|Total|A contagem de todos os falha chamadas de método direto.|
|c2d.methods.requestSize|Tamanho do pedido de invocações de método direto|Bytes|Média|A média, Mín e máx. de bem-sucedida de todos os pedidos de método direto.|
|c2d.methods.responseSize|Tamanho de resposta de invocações de método direto|Bytes|Média|A média, Mín e máx. de respostas de método direto tudo com êxito.|
|c2d.twin.read.success|Lê a partir do back-end duplo com êxito|Contagem|Total|Contagem de leituras de back-end-iniciada duplo tudo com êxito.|
|c2d.twin.read.failure|Leituras de duplo com falha de back-end|Contagem|Total|A contagem de todos os falha leituras duplo iniciada pelo back-end.|
|c2d.twin.read.size|Tamanho de resposta do duplo lê a partir do back-end|Bytes|Média|A média, Mín e máx. de todos os bem-sucedida iniciada pelo back-end duplo leituras.|
|c2d.twin.update.success|Duplo com êxito as atualizações a partir de back-end|Contagem|Total|Contagem de todos os com êxito atualizações de duplo iniciada pelo back-end.|
|c2d.twin.update.failure|Atualizações de duplo com falha de back-end|Contagem|Total|A contagem de todos os falha atualizações duplo iniciada pelo back-end.|
|c2d.twin.update.size|Tamanho de atualizações de duplo de back-end|Bytes|Média|A média, Mín e tamanho máximo de todos os bem-sucedida iniciada pelo back-end duplo atualizações.|
|twinQueries.success|Consultas de gémeos com êxito|Contagem|Total|Contagem de todas as consultas de gémeos com êxito.|
|twinQueries.failure|Consultas de falhados duplo|Contagem|Total|Contagem de todas as consultas de falhados duplo.|
|twinQueries.resultSize|Tamanho de resultados de consultas de gémeos|Bytes|Média|A média, Mín e máx. do tamanho do resultado de todas as consultas de gémeos com êxito.|
|jobs.createTwinUpdateJob.success|Criações com êxito das tarefas de atualização de duplo|Contagem|Total|Contagem de criação com êxito todas as tarefas de atualização de duplo.|
|jobs.createTwinUpdateJob.failure|Criações com falhas de tarefas de atualização de duplo|Contagem|Total|Contagem de todos os falha na criação de tarefas de atualização de duplo.|
|jobs.createDirectMethodJob.success|Criações com êxito das tarefas de invocação de método|Contagem|Total|Contagem de criação com êxito todas as tarefas de invocação do método direto.|
|jobs.createDirectMethodJob.failure|Criações com falhas de tarefas de invocação de método|Contagem|Total|Contagem de todos os falha na criação de tarefas de invocação do método direto.|
|jobs.listJobs.success|Chamadas com êxito à lista de tarefas|Contagem|Total|Contagem de todas as chamadas com êxito à lista de tarefas.|
|jobs.listJobs.failure|Falha de chamadas para a lista de tarefas|Contagem|Total|Contagem de todas as chamadas falhadas para lista de tarefas.|
|jobs.cancelJob.success|Cancelamentos de tarefas com êxito|Contagem|Total|Contagem de todas as chamadas com êxito para cancelar uma tarefa.|
|jobs.cancelJob.failure|Cancelamentos de tarefas com falhas|Contagem|Total|Contagem de todas as chamadas falhadas para cancelar uma tarefa.|
|jobs.queryJobs.success|Consultas de tarefa concluída com êxito|Contagem|Total|Contagem de todas as chamadas com êxito para tarefas de consulta.|
|jobs.queryJobs.failure|Consultas de tarefa falhadas|Contagem|Total|Contagem de todas as chamadas falhadas para tarefas de consulta.|
|Jobs.completed|Trabalhos concluídos|Contagem|Total|Contagem de todas as tarefas concluídas.|
|Jobs.Failed|Tarefas falhadas|Contagem|Total|Contagem de todas as tarefas falhadas.|

## <a name="next-steps"></a>Passos Seguintes
Agora que já viu uma descrição geral das métricas do IoT Hub, siga esta ligação para saber mais sobre como gerir o IoT Hub do Azure:

* [Monitorização de operações][lnk-monitor]

Para explorar ainda mais os recursos do IoT Hub, veja:

* [Guia do programador do IoT Hub][lnk-devguide]
* [Implementar o AI em dispositivos de ponta com o Azure IoT Edge][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png

[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
