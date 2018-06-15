---
title: Análise de lote do Azure | Microsoft Docs
description: Referência para a análise de lote do Azure.
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
ms.openlocfilehash: 4c81acb282b24bbd899227c4dcc449fed4ba6c7d
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
ms.locfileid: "30312682"
---
# <a name="batch-analytics"></a>Batch Analytics
Os tópicos na análise de lote contêm informações de referência para os eventos e alertas disponíveis para recursos do serviço Batch.

Consulte [registo de diagnóstico do Azure Batch](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) para obter mais informações sobre a ativação e o consumo de registos de diagnóstico do Batch.

## <a name="diagnostic-logs"></a>Registos de diagnósticos

O serviço Azure Batch emite os seguintes eventos de registo de diagnóstico durante a duração de certos recursos do Batch.

**Eventos de registo do serviço**
* [Criar conjunto](batch-pool-create-event.md)
* [Início de eliminação do conjunto](batch-pool-delete-start-event.md)
* [Eliminar conjunto concluída](batch-pool-delete-complete-event.md)
* [Início de redimensionamento de conjunto](batch-pool-resize-start-event.md)
* [Redimensionamento de agrupamento completo](batch-pool-resize-complete-event.md)
* [Início da tarefa](batch-task-start-event.md)
* [Tarefa concluída](batch-task-complete-event.md)
* [Falha de tarefa](batch-task-fail-event.md)