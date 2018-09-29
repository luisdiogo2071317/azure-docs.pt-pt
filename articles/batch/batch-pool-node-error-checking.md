---
title: Verificar a existência de erros de agrupamento e o nó do Batch
description: Erros para verificar a existência e como evitar durante a criação de conjuntos e nós
services: batch
author: mscurrell
ms.author: markscu
ms.date: 9/25/2018
ms.topic: conceptual
ms.openlocfilehash: bc09089fa9984e9042166938da19499afca21509
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435226"
---
# <a name="checking-for-pool-and-node-errors"></a>Verificar a existência de erros de agrupamento e o nó

Quando criar e gerir conjuntos do Batch, existem operações que imediatamente e existem assíncrona operações que não estão imediatas, execute em segundo plano e pode demorar vários minutos a concluir.

Detecção de falhas para operações que têm lugar imediatamente é simples, como a quaisquer falhas serão imediatamente devolvidas pela API, CLI ou da interface do Usuário.

Este artigo aborda as operações de segundo plano que podem ser feitas para conjuntos e nós de conjunto - Especifica como as falhas podem ser detectadas e como as falhas podem ser evitadas.

## <a name="pool-errors"></a>Erros de conjunto

### <a name="resize-timeout-or-failure"></a>Redimensionar o tempo limite ou de falha

Quando criar um novo conjunto ou redimensionar um conjunto existente, os números de destino de nós for especificado.  A operação será concluída imediatamente, mas a alocação real dos novos nós ou remoção de nós existentes ocorre em segundo plano por que poderá ser vários minutos.  Um tempo limite de redimensionamento é especificado na [crie](https://docs.microsoft.com/rest/api/batchservice/pool/add) ou [redimensionar](https://docs.microsoft.com/rest/api/batchservice/pool/resize) API - se o número de destino de nós não é possível obter no período de tempo limite de redimensionamento, em seguida, a operação irá parar com o pool vai para um estado estável e ver erros de redimensionamento.

Um tempo limite de redimensionamento é comunicado pelos [ResizeError](https://docs.microsoft.com/rest/api/batchservice/pool/get#resizeerror) propriedade para a última avaliação, que apresenta uma lista de um ou mais erros que ocorreram.

Causas comuns para tempos limite de redimensionamento incluem:
- Tempo limite de redimensionamento é demasiado curta
  - O tempo limite predefinido é 15 minutos, que é normalmente bastante tempo para nós do conjunto a ser alocados ou remoção.
  - Ao alocar um grande número de nós (mais de 1000 nós partir de uma imagem do Marketplace) ou mais de 300 nós a partir de uma imagem personalizada, durante a criação de conjunto ou de redimensionamento, em seguida, é recomendado um tempo limite de 30 minutos.
- Quota de núcleos suficiente
  - Uma conta do batch tem uma quota para o número de núcleos que ser alocados em todos os agrupamentos.  Batch não o aloca nós assim que esse quota foi atingido.  A quota de núcleos [pode ser aumentado](https://docs.microsoft.com/azure/batch/batch-quota-limit) para permitir que mais nós sejam alocados.
- IPs de sub-rede insuficiente quando um [conjunto estiver numa rede virtual](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - Uma sub-rede de rede virtual tem de ter suficiente não atribuídos os endereços IP para atribuir a todos os nós do conjunto a ser solicitados, caso contrário, os nós não não possível criar.
- Recursos insuficientes quando um [conjunto estiver numa rede virtual](https://docs.microsoft.com/azure/batch/batch-virtual-network)
  - Recursos, como balanceadores de carga, IPs públicos e NSGs são criados na subscrição utilizada para criar a conta do Batch.  As quotas de subscrição para estes recursos devem ser suficientes.
- Utilizar uma imagem VM personalizada para conjuntos grandes
  - Grandes pools usando imagens personalizadas pode demoram mais tempo a alocar e redimensione tempos limite pode ocorrer.  Limites e recomendações de configuração são fornecidas numa [artigo específico](https://docs.microsoft.com/azure/batch/batch-custom-images). 

### <a name="auto-scale-failures"></a>Falhas de dimensionamento automático

Em vez de definir explicitamente o número de destino de nós para um conjunto na criação do conjunto ou de redimensionamento, o número de nós num conjunto pode ser dimensionado automaticamente.  Uma [fórmula de dimensionamento automático pode ser criada para um conjunto](https://docs.microsoft.com/azure/batch/batch-automatic-scaling), que será avaliado num intervalo regular configurável para definir o número de destino de nós para o conjunto.  Os seguintes tipos de problemas podem ocorrer e detetado:

- A avaliação de dimensionamento automático pode falhar.
- A operação de redimensionamento resultante pode falhar e o tempo limite.
- Pode haver um problema com a fórmula de dimensionamento automático, que leva a valores de destino de nó incorreto, com o redimensionamento trabalhar ou exceder o tempo limite.

Informações sobre a última avaliação de dimensionamento automático são obtidas utilizando-o [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/pool/get#autoscalerun) propriedade, que os relatórios no momento da avaliação, os valores e o resultado da avaliação e quaisquer erros que realizar a avaliação.

Informações sobre todas as avaliações são capturadas automaticamente por uma [evento de conclusão de redimensionamento de conjunto](https://docs.microsoft.com/azure/batch/batch-pool-resize-complete-event).

### <a name="delete"></a>Eliminar

Partindo do princípio que há nós num conjunto, em seguida, eliminar de um conjunto de resultados da operação em nós de que está a ser eliminados primeiro, seguidos do próprio objeto de agrupamento.  Pode demorar alguns minutos para que os nós do conjunto ser eliminado.

O [estado do agrupamento](https://docs.microsoft.com/rest/api/batchservice/pool/get#poolstate) será definido como "eliminar" durante o processo de eliminação.  O aplicativo de chamada pode detectar se a eliminação de conjunto demorar demasiado tempo, usando as propriedades de "Estado" e 'stateTransitionTime'.

## <a name="pool-compute-node-errors"></a>Erros de nó de computação do conjunto

Nós podem ser alocados com êxito num conjunto, mas vários problemas podem levar a nós em mau estado de funcionamento e não ser utilizável.  Depois de nós são alocados num conjunto, eles incorrer em custos e, portanto, é importante detetar problemas para evitar pagar nós que não podem ser utilizados.

### <a name="start-task-failure"></a>Falha de tarefa de início

Opcional [iniciar tarefa](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask) pode ser especificado para um conjunto.  Tal como acontece com qualquer tarefa, podem ser especificados uma linha de comando e arquivos de recursos para transferir a partir do armazenamento.  A tarefa de início é especificada para o agrupamento, mas executadas em cada nó - assim que tiver sido iniciado a cada nó, em seguida, a tarefa de início é executada.  Uma propriedade adicional do [iniciar tarefa](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask), "waitForSuccess", especifica se Batch deve aguardar até a tarefa de início seja concluída com êxito antes de agendar quaisquer tarefas para um nó.

Se uma tarefa inicial falhar e a configuração de tarefa de início especificada para aguardar a conclusão com êxito, o nó não poderão ser utilizado e ainda vai incorrer encargos.

Falhas de tarefas de inicialização podem ser detectadas através da [resultado](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskexecutionresult) e [failureInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#taskfailureinformation) propriedades de nível superior [startTaskInfo](https://docs.microsoft.com/rest/api/batchservice/computenode/get#starttaskinformation) propriedades de nós.

Uma tarefa de início com falhas também leva para o nó [estado](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) a ser definido como 'starttaskfailed', mas apenas se 'waitForSuccess' foi definido como 'true'.

Tal como acontece com qualquer tarefa, podem existir muitas causas para a ativação da tarefa de início.  Para resolver problemas, devem ser verificados o stdout, stderr e quaisquer ficheiros de registo de tarefa específica ainda mais.

### <a name="application-package-download-failure"></a>Falha na transferência de pacote de aplicação

Um ou mais pacotes de aplicações, opcionalmente, podem ser especificados para um conjunto, com os ficheiros de pacote especificado que está a ser transferidos para cada nó e descomprimidos depois de iniciado o nó, mas antes das tarefas estão agendadas.  É comum para utilizar uma linha de comando de tarefa de início, em conjunto com pacotes de aplicações, para copiar ficheiros para uma localização diferente ou execute a configuração, por exemplo.

Uma falha ao transferir e descomprimir um pacote de aplicação será reportada pelo nó [erros](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) propriedade.  O estado do nó será definido como 'inutilizável'.

### <a name="node-in-unusable-state"></a>Nó de estado não utilizável

O [estado do nó](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodestate) pode ser definido para "inutilizável' por muitos motivos.  Quando "inutilizável," tarefas não podem ser agendadas para o nó, mas o nó ainda vai incorrer encargos.

Batch sempre tentará recuperar nós inutilizáveis, mas a recuperação pode ou não ser possível, dependendo da causa.

Onde é possível determinar a causa, será reportado pelo nó de [erros](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) propriedade.

Outros exemplos de causas para nós "inutilizáveis":

- Imagem personalizada inválida; não preparados corretamente, por exemplo.
- Falha de infra-estrutura ou atualização de baixo nível que leva a da VM subjacente, que está a ser movida; Batch irá recuperar o nó.

### <a name="node-agent-log-files"></a>Ficheiros de registo do agente de nó

Se for necessário contactar o suporte em relação a um problema de nó do conjunto, ficheiros de registo do processo de agente de lote que é executado em cada nó no conjunto podem ser obtidos.  Os ficheiros de registo para um nó podem ser carregados através do portal do Azure, o Explorador do Batch, ou uma [API](https://docs.microsoft.com/rest/api/batchservice/computenode/uploadbatchservicelogs).  Carregar e salvar os arquivos de log podem ser extremamente útil como o nó ou o conjunto pode ser eliminado para poupar no custo de nós em execução.

## <a name="next-steps"></a>Passos Seguintes

Certifique-se de que seu aplicativo implementou abrangente da verificação de erros, especialmente para operações assíncronas, para que podem ser rapidamente detetados e diagnosticados problemas.
