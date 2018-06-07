---
title: Desempenho e dimensionamento em funções durável - Azure
description: Introdução à extensão de funções durável para as funções do Azure.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 110f393e723c7e784a4bd7e79559dd9d55147140
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599437"
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Desempenho e dimensionamento em funções durável (funções do Azure)

Para otimizar o desempenho e escalabilidade, é importante compreender as características de dimensionamento exclusivas de [funções durável](durable-functions-overview.md).

Para compreender o comportamento de dimensionamento, tem de compreender alguns dos detalhes do fornecedor de armazenamento do Azure subjacente.

## <a name="history-table"></a>Tabela de histórico

O **histórico** tabela é uma tabela de armazenamento do Azure que contém os eventos de histórico para todas as instâncias de orquestração dentro de um concentrador de tarefas. O nome desta tabela tem o formato *TaskHubName*histórico. Como executam instâncias, novas linhas são adicionadas a esta tabela. A chave de partição desta tabela é derivada do ID de instância da orquestração. Um ID de instância é aleatório na maioria dos casos, que garante uma distribuição ideal de partições internas no armazenamento do Azure.

Quando uma instância de orquestração tem de executar, as linhas da tabela histórica adequadas são carregadas na memória. Estes *eventos histórico* , em seguida, são reproduzido para o código de função do orchestrator para obtê-lo novamente para o seu estado checkpointed anteriormente. A utilização do histórico de execução para reconstruir o estado desta forma é deve influenciada pelo [padrão de eventos Sourcing](https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing).

## <a name="instances-table"></a>Tabela de instâncias

O **instâncias** tabela é outra tabela de armazenamento do Azure que contém os Estados de todas as instâncias de orquestração dentro de um concentrador de tarefas. Como são criadas instâncias, novas linhas são adicionadas a esta tabela. A chave de partição desta tabela é o ID de instância de orquestração e a chave de linha é uma constante fixa. Há uma linha por instância de orquestração.

Esta tabela for utilizada para satisfazer os pedidos de consulta de instância do [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_System_String_) API, bem como o [consultas de estado HTTP API](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-http-api#get-instance-status). É mantida eventualmente consistente com o conteúdo a **histórico** tabela mencionado anteriormente. A utilização de uma tabela de armazenamento do Azure separada de forma eficiente satisfazer operações de consulta de instância desta forma é deve influenciada pelo [padrão do comando e segregação de responsabilidade de consulta (CQRS)](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs).

## <a name="internal-queue-triggers"></a>Acionadores de filas interno

Atividade e as funções do Orchestrator são acionadas por filas internas no hub de tarefas a aplicação de função. Se utilizar as filas desta forma fornece garantias de entrega de mensagem de "em menos uma" fiável. Existem dois tipos de filas em funções durável: o **fila controlo** e **fila de item de trabalho**.

### <a name="the-work-item-queue"></a>A fila de item de trabalho

Há uma fila de item de trabalho por hub tarefas nas funções durável. É uma fila básica e funciona de forma semelhante a qualquer outro `queueTrigger` fila das funções do Azure. Esta fila é utilizada para acionar sem monitorização de estado *funções de atividade* por dequeueing uma única mensagem cada vez. Cada uma destas mensagens contém entradas de função de atividade e metadados adicionais, tais como que função a executar. Quando uma aplicação de funções durável aumenta horizontalmente de forma a várias VMs, estas VMs todos os competem ter a aquisição de trabalho da fila de item de trabalho.

### <a name="control-queues"></a>Filas de controlo

Existem vários *controlar filas* por hub tarefas nas funções durável. A *fila controlo* é mais sofisticadas que a fila de item de trabalho mais simples. Controlo filas são utilizadas para acionar as funções de monitorização de estado do orchestrator. Porque as instâncias de função do orchestrator são singletons com monitorização de estado, não é possível utilizar um modelo de consumidor concorrente distribuam a carga entre VMs. Em vez disso, as mensagens do orchestrator são com balanceamento de carga entre as filas de controlo. Obter mais detalhes sobre este comportamento podem ser encontrados nas secções subsequentes.

Filas de controlo contêm uma variedade de tipos de mensagens de ciclo de vida de orquestração. Os exemplos incluem [mensagens de controlo do orchestrator](durable-functions-instance-management.md), a função de atividade *resposta* mensagens e mensagens do temporizador. Máximo de 32 mensagens serão removidas da fila controlo em inquéritos único. Estas mensagens contêm dados do payload, bem como metadados, incluindo destina-se de que instância de orquestração. Se várias mensagens dequeued destinam-se para a mesma instância de orquestração, serão processados como um lote.

## <a name="storage-account-selection"></a>Seleção de contas de armazenamento

As filas, tabelas e os blobs utilizados pelos durável funções são criados numa conta do Storage do Azure configurada. A conta a utilizar pode ser especificada utilizando o `durableTask/azureStorageConnectionStringName` definição **host.json** ficheiro.

```json
{
  "durableTask": {
    "azureStorageConnectionStringName": "MyStorageAccountAppSetting"
  }
}
```

Se não for especificado, a predefinição `AzureWebJobsStorage` é utilizada a conta de armazenamento. Para cargas de trabalho de desempenho sensíveis, no entanto, configurar uma conta de armazenamento predefinido não é recomendado. Funções duráveis utiliza armazenamento do Azure descontos elevados e através de uma conta de armazenamento dedicado isola a utilização do armazenamento de funções durável da utilização interna pelo anfitrião das funções do Azure.

## <a name="orchestrator-scale-out"></a>Escalamento horizontal do Orchestrator

Funções de atividade são sem monitorização de estado e expandido enviados automaticamente através da adição de VMs. As funções do Orchestrator, por outro lado, são *particionada* por um ou mais filas de controlo. O número de filas do controlo está definido no **host.json** ficheiro. Os seguintes conjuntos de fragmento host.json de exemplo a `durableTask/partitionCount` propriedade `3`.

```json
{
  "durableTask": {
    "partitionCount": 3
  }
}
```
Um concentrador de tarefas pode ser configurado com entre 1 e 16 partições. Se não for especificado, a contagem da partição predefinido é **4**.

Durante a ampliação várias instâncias de anfitrião de função (normalmente, num VMs diferentes), cada instância adquire um bloqueio das filas de controlo. Estes bloqueios internamente são implementados como concessões de armazenamento de BLOBs e certifique-se de que uma instância de orquestração só é executada numa instância de anfitrião único cada vez. Se um concentrador de tarefas estiver configurado com três filas de controlo, instâncias de orquestração podem ser com balanceamento de carga entre VMs máximo de três. VMs adicionais podem ser adicionadas para aumentar a capacidade de execução da função de atividade.

O diagrama seguinte ilustra como o anfitrião de funções do Azure interage com as entidades de armazenamento num ambiente expandido terminar.

![Diagrama de escala](media/durable-functions-perf-and-scale/scale-diagram.png)

Como é mostrado no diagrama anterior, todas as VMs competem para ter mensagens na fila de item de trabalho. No entanto, apenas três VMs podem adquirir mensagens de filas de controlo e cada VM bloqueia uma fila de controlo único.

As instâncias de orquestração estão distribuídas por todas as instâncias de fila de controlo. A distribuição é feita pelo ID de instância da orquestração de hash. Os IDs de instância por predefinição são GUIDs aleatórias, garantindo que as instâncias estão igualmente distribuídas todas as filas de controlo.

Um modo geral, as funções do orchestrator são se destina a ser simples e não devem requerer grandes quantidades de energia informática. Consequentemente, não é necessário criar um grande número de controlo de partições de fila para obter excelente débito. A maioria do trabalho pesada deve ser efetuada nas funções de atividade sem monitorização de estado, o que podem ser ampliadas infinitamente.

## <a name="auto-scale"></a>Dimensionamento Automático

Dado que todas as funções do Azure em execução no plano de consumo, funções durável suporta o dimensionamento automático através de [das funções do Azure escala controlador](functions-scale.md#runtime-scaling). O controlador de escala monitoriza a latência de todas as filas através da emissão de periodicamente _peek_ comandos. Com base nas latências das mensagens peeked, o controlador de escala irá decida se pretende adicionar ou remover as VMs.

Se o controlador de dimensionamento determina que são demasiado elevadas latências de mensagem de fila de controlo, irá adicionar as instâncias VM até que reduz a latência de mensagem para um nível aceitável ou atinge o número de partição de fila de controlo. Da mesma forma, o controlador de escala irá adicionar continuamente instâncias de VM se a fila de item de trabalho se elevada, independentemente da contagem de partição.

## <a name="thread-usage"></a>Utilização de thread

As funções do Orchestrator são executadas num único thread para se certificar de que execução pode ser determinista entre repetições muitos. Devido a execução deste tem thread único, é importante que threads de função do orchestrator não efetuar tarefas intensivas em CPU, não e/s ou bloquear por qualquer motivo. Qualquer conta que pode necessitar de e/s, bloquear, ou devem ser movidos de vários threads para funções de atividade.

As funções de atividade ter as mesmas comportamentos do regulares funções acionadas de fila. Em segurança que podem fazer e/s, executar operações intensivas de CPU e utilizar vários threads. Dado que os acionadores de atividade sem monitorização de estado, estes podem livremente aumentar horizontalmente para um número de VMs unbounded.

## <a name="concurrency-throttles"></a>Limitações de concorrência

As funções do Azure suporta a executar várias funções em simultâneo dentro de uma instância de aplicação único. A execução simultânea ajuda a aumentar o paralelismo e minimiza o número de "arranques" que uma aplicação típica irá ocorrer ao longo do tempo. No entanto, concorrência elevada pode resultar numa utilização de memória elevada-VM. Consoante as necessidades da aplicação de função, poderá ser necessário limitar a simultaneidade por instância para evitar a possibilidade de a executar o de memória em situações de carga elevada.

Ambos os atividade função orchestrator função simultaneidade limites e podem ser configurados no **host.json** ficheiro. As definições relevantes são `durableTask/maxConcurrentActivityFunctions` e `durableTask/maxConcurrentOrchestratorFunctions` respetivamente.

```json
{
  "durableTask": {
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
  }
}
```

No exemplo anterior, um máximo de 10 orchestrator e as funções 10 atividade pode ser executados numa única VM em simultâneo. Se não for especificado, o número de execuções de função de atividade e orchestrator em simultâneo é limitado a 10 X o número de núcleos na VM.

> [!NOTE]
> Estas definições são úteis para ajudar a gerir a memória e utilização da CPU de uma única VM. No entanto, quando ampliar através de várias VMs, cada VM terão o próprio conjunto de conjunto de limites. Estas definições não podem ser utilizadas para controlar a simultaneidade um nível global.

## <a name="orchestrator-function-replay"></a>Repetição de função do Orchestrator
Como mencionado anteriormente, as funções do orchestrator são reproduzidas utilizando os conteúdos do **histórico** tabela. Por predefinição, o código de função do orchestrator é reproduzido pela sempre que um lote de mensagens em fila são removida da fila de controlo.

Este comportamento de repetição agressiva pode ser desativado Ativando **expandido sessões**. Quando são ativadas sessões expandidas, instâncias de função do orchestrator são guardadas na memória mais e novas mensagens podem ser processadas sem uma repetição completa. Sessões expandidas estão ativadas por definição `durableTask/extendedSessionsEnabled` para `true` no **host.json** ficheiro. O `durableTask/extendedSessionIdleTimeoutInSeconds` definição é utilizada para controlar quanto será retida uma sessão inativa na memória:

```json
{
  "durableTask": {
    "extendedSessionsEnabled": true,
    "extendedSessionIdleTimeoutInSeconds": 30
  }
}
```

O efeito de ativação sessões expandidas típico é reduzido e/s contra a conta de armazenamento do Azure e o débito melhorado global.

No entanto, um downside potencial desta funcionalidade é essa função de inatividade do orchestrator instâncias permanecerão na memória mais. Existem dois efeitos a ter em consideração:

1. Geral aumento da utilização de memória da aplicação de função.
2. Em geral diminua no débito, se existirem várias execuções de função do orchestrator em simultâneo, curta duração.

Por exemplo, se `durableTask/extendedSessionIdleTimeoutInSeconds` está definido como 30 segundos, em seguida, um episódio de função de curta duração do orchestrator que é executada em menos de 1 segundo ainda vai ocupar memória durante 30 segundos. Também é a ser contabilizada contra o `durableTask/maxConcurrentOrchestratorFunctions` quota mencionada anteriormente, potencialmente a impedir que outras funções do orchestrator em execução.

> [!NOTE]
> Estas definições só devem ser utilizadas após uma função do orchestrator foi totalmente desenvolvida e testada. O comportamento de repetição agressiva predefinido é útil para detetar erros idempotency nas funções do orchestrator no momento de desenvolvimento.

## <a name="performance-targets"></a>Metas de desempenho

Quando planear utilizar funções durável para uma aplicação de produção, é importante a ter em consideração os requisitos de desempenho no início do processo de planeamento. Esta secção abrange alguns cenários de utilização básica e os números de débito máximo esperado.

* **Execução de actividade sequencial**: este cenário descreve uma função do orchestrator que executa uma série de funções de atividade um depois do outro. -Lo se assemelhe mais ao [função encadeamento](durable-functions-sequence.md) exemplo.
* **Atividade de execução paralela**: este cenário descreve uma função do orchestrator que executa a muitas funções de atividade em paralelo utilizando a [Fan-out, Fan-in](durable-functions-cloud-backup.md) padrão.
* **Resposta de processamento paralelo**: este cenário é a segunda metade do [Fan-out, Fan-in](durable-functions-cloud-backup.md) padrão. Concentra-se no desempenho do fan-in. É importante ter em atenção que, ao contrário fan-out, fan-in é feita por uma instância de função única orchestrator e, por conseguinte, só pode ser executada numa única VM.
* **O processamento de eventos externo**: este cenário representa uma instância de função do orchestrator único que aguarda no [eventos externos](durable-functions-external-events.md), um de cada vez.

> [!TIP]
> Ao contrário fan-out, operações fan-in estão limitadas a uma única VM. Se a aplicação utiliza o padrão de fan-in, fan-out e está preocupados sobre o desempenho de fan-in, considere subplano dividindo a fan-out de função da atividade em múltiplas [orchestrations secundárias](durable-functions-sub-orchestrations.md).

A tabela seguinte mostra a esperada *máximo* números de débito para os cenários descritos anteriormente. "Instâncias" refere-se a uma única instância de uma função do orchestrator em execução num único breve ([A1](../virtual-machines/windows/sizes-previous-gen.md#a-series)) VM no App Service do Azure. Em todos os casos, presume-se que [expandido sessões](#orchestrator-function-replay) estão ativadas. Resultados reais poderão variar consoante o trabalho de CPU ou e/s efetuado pelo código da função.

| Cenário | Débito máximo |
|-|-|
| Execução de actividade sequencial | 5 atividades por segundo, por instância |
| Execução de actividade paralela (fan-out) | 100 atividades por segundo, por instância |
| Processamento paralelo de resposta (fan-in) | 150 respostas por segundo, por instância |
| Processamento de eventos externo | 50 eventos por segundo, por instância |

> [!NOTE]
> Estes números estão atualizados na versão de v1.4.0 (GA) da extensão de funções durável. Estes números podem alterar ao longo do tempo, como a funcionalidade evoluiu e como as otimizações de serem efetuadas.

Se não vir os números de débito que espera e a CPU e utilização de memória é apresentado o bom estado de funcionamento, certifique-se a causa está relacionado com [o estado de funcionamento da sua conta de armazenamento](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#troubleshooting-guidance). As funções durável extensão pode colocar significativa de carga numa conta de armazenamento do Azure e suficientemente elevadas cargas pesadas poderão resultar na limitação de conta de armazenamento.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Instale a extensão de funções durável e amostras](durable-functions-install.md)
