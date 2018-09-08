---
title: Hubs de tarefas nas funções duráveis - Azure
description: Saiba o que um hub de tarefa é na extensão de funções duráveis para as funções do Azure. Saiba como configurar configurar hubs de tarefas.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 53c70d4407222a80a9bc948db51294cd3e4e1bb4
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092840"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Hubs de tarefas nas funções durável (funções do Azure)

R *hub da tarefa* na [funções duráveis](durable-functions-overview.md) é um contentor lógico para os recursos de armazenamento do Azure que são utilizadas para orquestrações. As funções do Orchestrator e atividade só podem interagir entre si quando eles pertencem ao mesmo hub de tarefa.

Cada aplicação de função tem um hub de tarefa separada. Se várias aplicações de funções partilharem uma conta de armazenamento, a conta de armazenamento contém diversos hubs de tarefas. O diagrama seguinte ilustra um hub de tarefa por aplicação de funções nas contas de armazenamento dedicados e partilhados.

![Diagrama que mostra partilhados e dedicados a contas de armazenamento.](media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Recursos de armazenamento do Azure

Um concentrador de tarefa inclui os seguintes recursos de armazenamento: 

* Uma ou mais filas de controle.
* Uma fila de item de trabalho.
* Tabela de histórico de um.
* Uma tabela de instâncias.
* Um contentor de armazenamento que contém um ou mais blobs de concessão.

Todos esses recursos são criados automaticamente na conta de armazenamento do Azure do padrão quando o orchestrator ou funções de atividade executarem ou são agendadas para serem executadas. O [desempenho e dimensionamento](durable-functions-perf-and-scale.md) artigo explica como esses recursos são usados.

## <a name="task-hub-names"></a>Nomes de hubs de tarefas

Hubs de tarefas são identificados por um nome que é declarado no *Host. JSON* ficheiro, conforme mostrado no exemplo a seguir:

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Os nomes de hubs de tarefas devem começar com uma letra e consistir apenas letras e números. Se não for especificado, o nome predefinido é **DurableFunctionsHub**.

> [!NOTE]
> O nome é o que diferencia um hub de tarefa de outro quando existem diversos hubs de tarefas numa conta de armazenamento partilhado. Se tiver várias aplicações function App, compartilhamento de uma conta de armazenamento partilhado, tem de configurar nomes diferentes para cada hub de tarefas no *Host. JSON* ficheiros.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saber como lidar com controlo de versões](durable-functions-versioning.md)
