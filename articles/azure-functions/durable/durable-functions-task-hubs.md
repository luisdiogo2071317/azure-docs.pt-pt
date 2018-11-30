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
ms.openlocfilehash: 7a6346e594c5a7cc4cf02f3ea658aac4977e641a
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642621"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Hubs de tarefas nas funções durável (funções do Azure)

R *hub da tarefa* na [funções duráveis](durable-functions-overview.md) é um contentor lógico para os recursos de armazenamento do Azure que são utilizadas para orquestrações. As funções do Orchestrator e atividade só podem interagir entre si quando eles pertencem ao mesmo hub de tarefa.

Cada aplicação de função tem um hub de tarefa separada. Se várias aplicações de funções partilharem uma conta de armazenamento, a conta de armazenamento contém diversos hubs de tarefas. O diagrama seguinte ilustra um hub de tarefa por aplicação de funções nas contas de armazenamento dedicados e partilhados.

![Diagrama que mostra partilhados e dedicados a contas de armazenamento.](./media/durable-functions-task-hubs/task-hubs-storage.png)

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

### <a name="hostjson-functions-v1"></a>o Host. JSON (funções v1)
```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```
### <a name="hostjson-functions-v2"></a>o Host. JSON (funções v2)
```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "HubName": "MyTaskHub"
    }
  }
}
```
Hubs de tarefas também podem ser configurados com as definições de aplicação, conforme mostrado a seguir *Host. JSON* exemplo de ficheiro:

### <a name="hostjson-functions-v1"></a>o Host. JSON (funções v1)
```json
{
  "durableTask": {
    "HubName": "%MyTaskHub%"
  }
}
```
### <a name="hostjson-functions-v2"></a>o Host. JSON (funções v2)
```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "HubName": "%MyTaskHub%"
    }
  }
}
```
O nome do hub de tarefas será definido para o valor da `MyTaskHub` definição de aplicação. O seguinte procedimento `local.settings.json` demonstra como definir o `MyTaskHub` definir como `samplehubname`:

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" :  "samplehubname" 
  }
}
```

Os nomes de hubs de tarefas devem começar com uma letra e consistir apenas letras e números. Se não for especificado, o nome predefinido é **DurableFunctionsHub**.

> [!NOTE]
> O nome é o que diferencia um hub de tarefa de outro quando existem diversos hubs de tarefas numa conta de armazenamento partilhado. Se tiver várias aplicações function App, compartilhamento de uma conta de armazenamento partilhado, tem de configurar nomes diferentes para cada hub de tarefas no *Host. JSON* ficheiros.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saber como lidar com controlo de versões](durable-functions-versioning.md)
