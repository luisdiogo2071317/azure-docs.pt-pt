---
title: Hubs de tarefas nas funções durável - Azure
description: Saiba que um concentrador de tarefas é na extensão do durável funções para as funções do Azure. Saiba como configurar configurar os hubs de tarefas.
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
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 563667684accf8b434052cd412bf6e93c77ea63a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Hubs de tarefas nas funções durável (funções do Azure)

A *hub tarefas* no [funções durável](durable-functions-overview.md) é um contentor lógico para os recursos de armazenamento do Azure que são utilizados para orchestrations. As funções do Orchestrator e atividade só podem interagir com entre si quando pertencem ao mesmo hub de tarefas.

Cada aplicação de função tem um concentrador de tarefa separada. Se várias aplicações de função partilharem uma conta de armazenamento, a conta de armazenamento contém vários hubs de tarefas. O diagrama seguinte ilustra um hub de tarefas por aplicação de função em contas de armazenamento partilhado e dedicado.

![Apresentação do diagrama partilhados e dedicados contas de armazenamento.](media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Recursos de armazenamento do Azure

Um concentrador de tarefas consiste dos seguintes recursos de armazenamento: 

* Uma ou mais filas de controlo.
* Uma fila de item de trabalho.
* Tabela de histórico de um.
* Uma tabela de instâncias.
* Um contentor de armazenamento que contém um ou mais blobs de concessão.

Todos estes recursos são criados automaticamente na conta do Storage do Azure predefinido ao orchestrator ou funções de atividade executam ou que estejam agendadas para execução. O [desempenho e dimensionamento](durable-functions-perf-and-scale.md) artigo explica como estes recursos são utilizados.

## <a name="task-hub-names"></a>Nomes de hub de tarefas

Hubs de tarefas são identificados por um nome que está declarado no *host.json* ficheiro, conforme mostrado no exemplo seguinte:

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Os nomes de hub de tarefas tem de começar com uma letra e incluir apenas letras e números. Se não for especificado, o nome predefinido é **DurableFunctionsHub**.

> [!NOTE]
> O nome é que um hub de tarefa diferencia de à outra quando existem vários hubs de tarefas numa conta de armazenamento partilhado. Se tiver várias aplicações de função partilha uma conta de armazenamento partilhado, tem de configurar diferentes nomes para cada hub tarefas a *host.json* ficheiros.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saber como lidar com controlo de versões](durable-functions-versioning.md)
