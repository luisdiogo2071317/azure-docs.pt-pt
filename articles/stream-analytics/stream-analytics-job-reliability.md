---
title: Evitar interrupções do serviço nas tarefas do Azure Stream Analytics
description: Este artigo descreve as orientações no efetuar as tarefas do Stream Analytics atualizar resiliente.
services: stream-analytics
author: jseb225
manager: kfile
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 47ccfe99d2ee6576dbb70324eb383f52d2a1b2e7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30902725"
---
# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Garante fiabilidade de tarefa do Stream Analytics durante as atualizações de serviço

Parte do que está a ser um serviço completamente gerido é a capacidade para introduzir novas funcionalidades de serviço e melhoramentos a um ritmo rápido. Como resultado, o Stream Analytics pode ter uma atualização de serviço implementar numa base semanal (ou mais frequentes). Independentemente da quantidade de teste é efetuada há ainda um risco de uma tarefa existente, em execução poderá interromper devido à introdução de um erro. Para os clientes que executam tarefas de processamento de transmissão em fluxo críticos estes riscos tem de ser evitadas. Um mecanismo que os clientes podem utilizar para reduzir este risco é do Azure **[região emparelhado](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** modelo. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Como é que regiões do Azure de emparelhado lidar com esta preocupação?

Do Stream Analytics garante que as tarefas em regiões emparelhadas são atualizadas em lotes separados. Como resultado é um intervalo de tempo suficiente entre as atualizações para identificar potenciais erros de quebra e remedeiam-nos.

_Índia Central com a exceção_ (cujo região emparelhada, Sul da Índia e não tiver a presença de Stream Analytics), a implementação de uma atualização do Stream Analytics não ocorreriam ao mesmo tempo num conjunto de regiões emparelhados. Implementações em várias regiões **no mesmo grupo** podem ocorrer **ao mesmo tempo**.

O artigo **[disponibilidade e regiões emparelhadas](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** tem as informações mais atualizadas em que regiões estão emparelhadas.

Os clientes aconselhados implementar tarefas idênticas em ambas as regiões emparelhadas. Para além do Stream Analytics interno capacidades de monitorização, os clientes também aconselha-se para monitorizar as tarefas como se **ambos** tarefas de produção. Se for identificada uma quebra de ser um resultado da atualização de serviço do Stream Analytics, escalar corretamente e efetuar a ativação pós-falha de quaisquer consumidores a jusante para o resultado da tarefa em bom estado. Escalamento de suporte irá impedir que a região emparelhada sejam afetados pela nova implementação e manter a integridade das tarefas de emparelhado.
