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
ms.openlocfilehash: b1c9854d794c88c28c82a4b6e40c81a29552223a
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984080"
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

|Métrica|Nome a apresentar de métrica|Unidade|Tipo de Agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Tentativas de envio de mensagens de telemetria|Contagem|Total|Número de mensagens de telemetria do dispositivo-para-cloud tentada a serem enviados ao seu hub IoT|Nenhuma dimensão|
|d2c.telemetry.ingress.success|Mensagens de telemetria enviadas|Contagem|Total|Número de mensagens de telemetria do dispositivo-para-cloud enviado com êxito ao seu hub IoT|Nenhuma dimensão|
|c2d.commands.egress.complete.success|Comandos concluídos|Contagem|Total|Número de comandos do cloud-para-dispositivo foi concluída com êxito pelo dispositivo|Nenhuma dimensão|
|c2d.commands.egress.abandon.success|Comandos abandonados|Contagem|Total|Número de comandos do cloud-para-dispositivo abandonadas pelo dispositivo|Nenhuma dimensão|
|c2d.commands.egress.reject.success|Comandos rejeitados|Contagem|Total|Número de comandos do cloud-para-dispositivo rejeitado pelo dispositivo|Nenhuma dimensão|
|devices.totalDevices|Total de dispositivos (preterido)|Contagem|Total|Número de dispositivos registados no seu hub IoT|Nenhuma dimensão|
|devices.connectedDevices.allProtocol|Dispositivos ligados (preteridos) |Contagem|Total|Número de dispositivos ligados ao seu hub IoT|Nenhuma dimensão|
|d2c.telemetry.egress.success|Encaminhamento: mensagens de telemetria entregues|Contagem|Total|O número de vezes que as mensagens foram entregues com êxito a todos os pontos finais através do IoT Hub encaminhamento. Se uma mensagem é encaminhada para vários pontos de extremidade, esse valor aumenta um para cada entrega concluída com êxito. Se uma mensagem é enviada para o mesmo ponto final várias vezes, esse valor aumenta um para cada entrega concluída com êxito.|Nenhuma dimensão|
|d2c.telemetry.egress.dropped|Encaminhamento: mensagens de telemetria removidas |Contagem|Total|O número de vezes que as mensagens foram removidas pelo IoT Hub encaminhamento devido a pontos de extremidade inativos. Este valor não conta mensagens entregues a rota de contingência, como mensagens de ignorados não são entregues lá.|Nenhuma dimensão|
|d2c.telemetry.egress.orphaned|Encaminhamento: mensagens de telemetria órfãos |Contagem|Total|O número de vezes que as mensagens foram órfão pelo encaminhamento do IoT Hub porque eles não correspondam a quaisquer regras de encaminhamento (incluindo a regra de contingência). |Nenhuma dimensão|
|d2c.telemetry.egress.invalid|Encaminhamento: mensagens de telemetria incompatíveis|Contagem|Total|O número de vezes que o IoT Hub encaminhamento não foi possível entregar mensagens devido a uma incompatibilidade com o ponto final. Este valor não inclui tentativas.|Nenhuma dimensão|
|d2c.telemetry.egress.fallback|Encaminhamento: mensagens entregues para contingência|Contagem|Total|O número de vezes que o IoT Hub encaminhamento entregar mensagens para o ponto final associado a rota de contingência.|Nenhuma dimensão|
|d2c.endpoints.egress.eventHubs|Encaminhamento: mensagens entregues ao Hub de eventos|Contagem|Total|O número de vezes que o IoT Hub com êxito de encaminhamento entregar mensagens para pontos finais do Hub de eventos.|Nenhuma dimensão|
|d2c.endpoints.latency.eventHubs|Encaminhamento: latência da mensagem para o Hub de eventos|Milissegundos|Média|A latência média (milissegundos) entre a receção de mensagem para o IoT Hub e entrada de mensagem num ponto de extremidade do Hub de eventos.|Nenhuma dimensão|
|d2c.endpoints.egress.serviceBusQueues|Encaminhamento: mensagens entregues à fila do Service Bus|Contagem|Total|O número de vezes que o IoT Hub com êxito de encaminhamento entregar mensagens para pontos finais de fila do Service Bus.|Nenhuma dimensão|
|d2c.endpoints.latency.serviceBusQueues|Encaminhamento: latência de mensagem de fila do Service Bus|Milissegundos|Média|A latência média (milissegundos) entre a receção de mensagem para o IoT Hub e entrada de mensagem de telemetria para um ponto de extremidade de fila do Service Bus.|Nenhuma dimensão|
|d2c.endpoints.egress.serviceBusTopics|Encaminhamento: mensagens entregues ao tópico do Service Bus|Contagem|Total|O número de vezes encaminhamento com êxito o IoT Hub entregar mensagens para pontos finais de tópico do Service Bus.|Nenhuma dimensão|
|d2c.endpoints.latency.serviceBusTopics|Encaminhamento: latência da mensagem para o tópico do Service Bus|Milissegundos|Média|A latência média (milissegundos) entre a receção de mensagem para o IoT Hub e entrada de mensagem de telemetria para um ponto de extremidade do tópico do Service Bus.|Nenhuma dimensão|
|d2c.endpoints.egress.builtIn.events|Encaminhamento: mensagens entregues para mensagens/eventos|Contagem|Total|O número de vezes que o IoT Hub com êxito de encaminhamento entregar mensagens para o ponto final incorporado (mensagens/eventos).|Nenhuma dimensão|
|d2c.endpoints.latency.builtIn.events|Encaminhamento: latência da mensagem para mensagens/eventos|Milissegundos|Média|A latência média (milissegundos) entre a receção de mensagem para o IoT Hub e entrada de mensagem de telemetria para o ponto final incorporado (mensagens/eventos).|Nenhuma dimensão|
|d2c.endpoints.egress.storage|Encaminhamento: mensagens entregues para o armazenamento|Contagem|Total|O número de vezes encaminhamento com êxito o IoT Hub entregar mensagens para pontos finais de armazenamento.|Nenhuma dimensão|
|d2c.endpoints.latency.storage|Encaminhamento: latência da mensagem para armazenamento|Milissegundos|Média|A latência média (milissegundos) entre a receção de mensagem para o IoT Hub e entrada de mensagem de telemetria para um ponto de final de armazenamento.|Nenhuma dimensão|
|d2c.endpoints.egress.storage.bytes|Encaminhamento: dados entregues para o armazenamento|Bytes|Total|A quantidade de dados (bytes) IoT Hub encaminhamento entregues aos pontos finais de armazenamento.|Nenhuma dimensão|
|d2c.endpoints.egress.storage.blobs|Encaminhamento: blobs é entregue ao armazenamento|Contagem|Total|O número de vezes que o IoT Hub encaminhamento entregue blobs para pontos finais de armazenamento.|Nenhuma dimensão|
|d2c.twin.read.success|Lê a partir do dispositivos duplos com êxito|Contagem|Total|Contagem de leituras de iniciada pelo dispositivo duplo tudo com êxito.|Nenhuma dimensão|
|d2c.twin.read.failure|Falha de leituras de twin de dispositivos|Contagem|Total|A contagem de todos os falha leituras iniciada pelo dispositivo duplo.|Nenhuma dimensão|
|d2c.twin.read.size|Tamanho de resposta de leituras de twin de dispositivos|Bytes|Média|A média, Mín e máx. de tudo com êxito iniciada pelo dispositivo duplo leituras.|Nenhuma dimensão|
|d2c.twin.update.success|Atualizações de duplo com êxito a partir de dispositivos|Contagem|Total|Contagem de todos os com êxito atualizações de iniciada pelo dispositivo duplo.|Nenhuma dimensão|
|d2c.twin.update.failure|Falha de atualizações de twin de dispositivos|Contagem|Total|A contagem de todos os falha atualizações iniciadas pelo dispositivo duplo.|Nenhuma dimensão|
|d2c.twin.update.size|Tamanho de atualizações de twin de dispositivos|Bytes|Média|A média, Mín e tamanho máximo de todos os bem-sucedida iniciada pelo dispositivo duplo atualizações.|Nenhuma dimensão|
|c2d.methods.Success|Invocações de método direto com êxito|Contagem|Total|Contagem de chamadas de método direto tudo com êxito.|Nenhuma dimensão|
|c2d.methods.Failure|Falha de invocações de método direto|Contagem|Total|A contagem de todos os falha chamadas de método direto.|Nenhuma dimensão|
|c2d.methods.requestSize|Tamanho do pedido de invocações de método direto|Bytes|Média|A média, Mín e máx. de bem-sucedida de todos os pedidos de método direto.|Nenhuma dimensão|
|c2d.methods.responseSize|Tamanho de resposta de invocações de método direto|Bytes|Média|A média, Mín e máx. de respostas de método direto tudo com êxito.|Nenhuma dimensão|
|c2d.twin.read.success|Lê a partir do back-end duplo com êxito|Contagem|Total|Contagem de leituras de back-end-iniciada duplo tudo com êxito.|Nenhuma dimensão|
|c2d.twin.read.failure|Leituras de duplo com falha de back-end|Contagem|Total|A contagem de todos os falha leituras duplo iniciada pelo back-end.|Nenhuma dimensão|
|c2d.twin.read.size|Tamanho de resposta do duplo lê a partir do back-end|Bytes|Média|A média, Mín e máx. de todos os bem-sucedida iniciada pelo back-end duplo leituras.|Nenhuma dimensão|
|c2d.twin.update.success|Duplo com êxito as atualizações a partir de back-end|Contagem|Total|Contagem de todos os com êxito atualizações de duplo iniciada pelo back-end.|Nenhuma dimensão|
|c2d.twin.update.failure|Atualizações de duplo com falha de back-end|Contagem|Total|A contagem de todos os falha atualizações duplo iniciada pelo back-end.|Nenhuma dimensão|
|c2d.twin.update.size|Tamanho de atualizações de duplo de back-end|Bytes|Média|A média, Mín e tamanho máximo de todos os bem-sucedida iniciada pelo back-end duplo atualizações.|Nenhuma dimensão|
|twinQueries.success|Consultas de gémeos com êxito|Contagem|Total|Contagem de todas as consultas de gémeos com êxito.|Nenhuma dimensão|
|twinQueries.failure|Consultas de falhados duplo|Contagem|Total|Contagem de todas as consultas de falhados duplo.|Nenhuma dimensão|
|twinQueries.resultSize|Tamanho de resultados de consultas de gémeos|Bytes|Média|A média, Mín e máx. do tamanho do resultado de todas as consultas de gémeos com êxito.|Nenhuma dimensão|
|jobs.createTwinUpdateJob.success|Criações com êxito das tarefas de atualização de duplo|Contagem|Total|Contagem de criação com êxito todas as tarefas de atualização de duplo.|Nenhuma dimensão|
|jobs.createTwinUpdateJob.failure|Criações com falhas de tarefas de atualização de duplo|Contagem|Total|Contagem de todos os falha na criação de tarefas de atualização de duplo.|Nenhuma dimensão|
|jobs.createDirectMethodJob.success|Criações com êxito das tarefas de invocação de método|Contagem|Total|Contagem de criação com êxito todas as tarefas de invocação do método direto.|Nenhuma dimensão|
|jobs.createDirectMethodJob.failure|Criações com falhas de tarefas de invocação de método|Contagem|Total|Contagem de todos os falha na criação de tarefas de invocação do método direto.|Nenhuma dimensão|
|jobs.listJobs.success|Chamadas com êxito à lista de tarefas|Contagem|Total|Contagem de todas as chamadas com êxito à lista de tarefas.|Nenhuma dimensão|
|jobs.listJobs.failure|Falha de chamadas para a lista de tarefas|Contagem|Total|Contagem de todas as chamadas falhadas para lista de tarefas.|Nenhuma dimensão|
|jobs.cancelJob.success|Cancelamentos de tarefas com êxito|Contagem|Total|Contagem de todas as chamadas com êxito para cancelar uma tarefa.|Nenhuma dimensão|
|jobs.cancelJob.failure|Cancelamentos de tarefas com falhas|Contagem|Total|Contagem de todas as chamadas falhadas para cancelar uma tarefa.|Nenhuma dimensão|
|jobs.queryJobs.success|Consultas de tarefa concluída com êxito|Contagem|Total|Contagem de todas as chamadas com êxito para tarefas de consulta.|Nenhuma dimensão|
|jobs.queryJobs.failure|Consultas de tarefa falhadas|Contagem|Total|Contagem de todas as chamadas falhadas para tarefas de consulta.|Nenhuma dimensão|
|Jobs.completed|Trabalhos concluídos|Contagem|Total|Contagem de todas as tarefas concluídas.|Nenhuma dimensão|
|Jobs.Failed|Tarefas falhadas|Contagem|Total|Contagem de todas as tarefas falhadas.|Nenhuma dimensão|
|d2c.telemetry.ingress.sendThrottle|Número de erros de limitação|Contagem|Total|Limita o número de erros de limitação devido ao débito de dispositivo|Nenhuma dimensão|
|dailyMessageQuotaUsed|Número total de mensagens utilizada|Contagem|Média|Número de total de mensagens usados hoje em dia. Este é um valor cumulativo que é reposto a zero 00:00 UTC todos os dias.|Nenhuma dimensão|
|deviceDataUsage|Utilização total devicedata (preterida)|Bytes|Total|Bytes transferidos de e para quaisquer dispositivos ligados ao IotHub|Nenhuma dimensão|
|deviceDataUsageV2|Utilização de dados do total de dispositivos (pré-visualização)|Bytes|Total|Bytes transferidos de e para quaisquer dispositivos ligados ao IotHub|Nenhuma dimensão|
|totalDeviceCount|Total de dispositivos (pré-visualização)|Contagem|Média|Número de dispositivos registados no seu hub IoT|Nenhuma dimensão|
|connectedDeviceCount|Dispositivos ligados (pré-visualização)|Contagem|Média|Número de dispositivos ligados ao seu hub IoT|Nenhuma dimensão|
|Configurações|Métricas de configuração|Contagem|Total|Métricas para operações de configuração|Nenhuma dimensão|

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
