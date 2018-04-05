---
title: Eventos de início de redimensionamento de agrupamento do Azure Batch | Microsoft Docs
description: Referência para o evento de início de redimensionamento de conjunto de Batch.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: b4412764c313669dc154fccaa56f22417262994d
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="pool-resize-start-event"></a>Evento de início de redimensionamento de conjunto

 Este evento é emitido quando um conjunto de redimensionamento foi iniciado. Uma vez que o redimensionamento de agrupamento um evento assíncrono, que pode esperar um evento de completa de redimensionamento de agrupamento para ser emitidos uma vez concluída a operação de redimensionamento.

 O exemplo seguinte mostra o corpo de um evento de início de redimensionamento de agrupamento para um conjunto redimensionamento de 0 a 2 nós com um redimensionamento manual.

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|poolId|Cadeia|O id do conjunto.|
|nodeDeallocationOption|Cadeia|Especifica quando nós podem ser removidos do conjunto, se estiver a diminuir o tamanho do conjunto.<br /><br /> Os valores possíveis são:<br /><br /> **requeue** – terminar as tarefas em execução e requeue-los. As tarefas serão executadas novamente quando a tarefa está ativada. Remova nós assim que as tarefas forem terminadas.<br /><br /> **terminar** – terminar as tarefas em execução. As tarefas não serão executadas novamente. Remova nós assim que as tarefas forem terminadas.<br /><br /> **taskcompletion** – permitir que as tarefas atualmente em execução para concluir. Não agende novas tarefas ao aguardar a resposta. Remova nós quando todas as tarefas estiverem concluídas.<br /><br /> **Retaineddata** -permitir atualmente em execução das tarefas de conclusão, em seguida, aguarde que todas as tarefas períodos de retenção de dados a expirar. Não agende novas tarefas ao aguardar a resposta. Remova nós quando todos os períodos de retenção de tarefas tem expirado.<br /><br /> O valor predefinido é requeue.<br /><br /> Se estiver a aumentar o tamanho do conjunto, em seguida, o valor é definido como **inválido**.|
|currentDedicated|Int32|O número de nós de computação atualmente atribuídos ao agrupamento.|
|targetDedicated|Int32|O número de nós de computação que são pedidos para o conjunto.|
|enableAutoScale|Booleano|Especifica se o tamanho do conjunto ajusta automaticamente ao longo do tempo.|
|isAutoPool|Booleano|Speficies se o conjunto foi criado através do mecanismo de AutoPool de uma tarefa.|
