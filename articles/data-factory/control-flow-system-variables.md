---
title: Variáveis do sistema no Azure Data Factory | Microsoft Docs
description: Este artigo descreve as variáveis de sistema suportadas pelo Azure Data Factory. Pode utilizar estas variáveis em expressões para definir a entidades do Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: shlo
ms.openlocfilehash: 43ea8703bdbfc23511c831a5f91c9461948cc254
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059186"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Variáveis de sistema suportadas pelo Azure Data Factory
Este artigo descreve as variáveis de sistema suportadas pelo Azure Data Factory. Pode utilizar estas variáveis em expressões para definir a entidades do Data Factory.

## <a name="pipeline-scope"></a>Âmbito de pipeline
Estas variáveis de sistema podem ser referenciadas em qualquer lugar no JSON do pipeline.

| Nome da variável | Descrição |
| --- | --- |
| @pipeline().DataFactory |Nome da fábrica de dados do pipeline executar está em execução no |
| @pipeline().Pipeline |Nome do pipeline |
| @pipeline().RunId | ID do pipeline específico executar |
| @pipeline().TriggerType | Tipo de accionador que invocar o pipeline (Manual, programador) |
| @pipeline().TriggerId| ID do acionador que invoca o pipeline |
| @pipeline().TriggerName| Nome do acionador que invoca o pipeline |
| @pipeline().TriggerTime| Hora em que o acionador invocado o pipeline. O tempo de Acionador é a hora real fired, não da data agendada. Por exemplo, `13:20:08.0149599Z` é devolvido em vez de `13:20:00.00Z` |

## <a name="schedule-trigger-scope"></a>Âmbito de Acionador de agenda
Estas variáveis de sistema podem ser referenciadas em qualquer lugar no acionador JSON se o acionador não é do tipo: "ScheduleTrigger."

| Nome da variável | Descrição |
| --- | --- |
| @trigger.scheduledTime) |Hora quando o acionador foi agendado para invocar o pipeline de executar. Por exemplo, para um acionador desencadeado a cada 5 min, esta variável devolvam `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z`, `2017-06-01T22:29:00Z` respetivamente.|
| @trigger.startTime) |Hora em que o acionador **realmente** é desencadeado para invocar o pipeline de executar. Por exemplo, para um acionador desencadeado a cada 5 min, esta variável pode devolver algo semelhante ao seguinte `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z`, `2017-06-01T22:29:00.9935483Z` respetivamente.|

## <a name="tumbling-window-trigger-scope"></a>Âmbito de Acionador de janela em cascata
Estas variáveis de sistema podem ser referenciadas em qualquer lugar no acionador JSON se o acionador não é do tipo: "TumblingWindowTrigger."

| Nome da variável | Descrição |
| --- | --- |
| @trigger(). outputs.windowStartTime |Início da janela quando o acionador foi agendado para invocar a execução de pipeline. Se o acionador de janela em cascata tem uma frequência de "hora a hora" é a hora no início da hora.|
| @trigger(). outputs.windowEndTime |Fim da janela quando o acionador foi agendado para invocar o pipeline de executar. Se o acionador de janela em cascata tem uma frequência de "hora a hora" é a hora no fim da hora.|
## <a name="next-steps"></a>Passos Seguintes
Para obter informações sobre como estas variáveis são utilizadas em expressões, consulte [linguagem de expressão e funções](control-flow-expression-language-functions.md).
