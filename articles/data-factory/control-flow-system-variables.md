---
title: "Variáveis do sistema no Azure Data Factory | Microsoft Docs"
description: "Este artigo descreve as variáveis de sistema suportadas pelo Azure Data Factory. Pode utilizar estas variáveis em expressões para definir a entidades do Data Factory."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: bdf1754226852145e9bf5597256339549f253071
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Variáveis de sistema suportadas pelo Azure Data Factory
Este artigo descreve as variáveis de sistema suportadas pelo Azure Data Factory. Pode utilizar estas variáveis em expressões para definir a entidades do Data Factory. 

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [funções e as variáveis no Data Factory versão 1](v1/data-factory-functions-variables.md).


## <a name="pipeline-scope"></a>Âmbito de pipeline:

| Nome da variável | Descrição |
| --- | --- |
| @pipeline().DataFactory |Nome da fábrica de dados do pipeline executar está em execução no | 
| @pipeline().Pipeline |Nome do pipeline |
| @pipeline().RunId | ID do pipeline específico executar | 
| @pipeline().TriggerType | Tipo de accionador que invocar o pipeline (Manual, programador) | 
| @pipeline().TriggerId| ID do acionador que invoca o pipeline |
| @pipeline().TriggerName| Nome do acionador que invoca o pipeline |
| @pipeline().TriggerTime| Hora em que o acionador invocado o pipeline. O tempo de Acionador é a hora real fired, não da data agendada. Por exemplo, `13:20:08.0149599Z` é devolvido em vez de`13:20:00.00Z` |

## <a name="trigger-scope"></a>Âmbito do acionador:

| Nome da variável | Descrição |
| --- | --- |
| trigger().scheduledTime |Hora quando o acionador foi agendado para invocar o pipeline de executar. Por exemplo, para um acionador desencadeado a cada 5 min, esta variável devolvam `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z`, `2017-06-01T22:29:00Z` respetivamente.|
| trigger().startTime |Hora em que o acionador **realmente** é desencadeado para invocar o pipeline de executar. Por exemplo, para um acionador desencadeado a cada 5 min, esta variável pode devolver algo semelhante ao seguinte `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z`, `2017-06-01T22:29:00.9935483Z` respetivamente.|

## <a name="next-steps"></a>Passos Seguintes
Para obter informações sobre como estas variáveis são utilizadas em expressões, consulte [linguagem de expressão e funções](control-flow-expression-language-functions.md). 
