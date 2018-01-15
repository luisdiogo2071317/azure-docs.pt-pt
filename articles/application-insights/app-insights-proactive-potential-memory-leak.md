---
title: "Smart deteção - potencial fuga de memória detetadas pelo Azure Application Insights | Microsoft Docs"
description: "Monitorizar aplicações com o Azure Application Insights para potenciais fugas de memória."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: e98caaa387418d746905990436b69925a591b260
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="memory-leak-detection-preview"></a>Deteção de fuga de memória (pré-visualização)

Application Insights automaticamente analisa o consumo de memória de cada processo na sua aplicação e pode avisá-lo sobre potenciais fugas de memória ou o consumo de memória de aumento.

Esta funcionalidade não necessita de nenhuma configuração especial, diferente de [configurar os contadores de desempenho](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) para a sua aplicação. Está ativa quando a sua aplicação gerar suficiente telemetria de contadores de desempenho de memória (por exemplo, Bytes privados).


## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Quando receber este tipo de notificação de deteção inteligente?
Uma notificação típica seguirá um aumento consistente no consumo de memória durante um longo período de tempo (algumas horas) num ou mais processos e/ou de uma ou mais máquinas que fazem parte da sua aplicação.
Algoritmos de Machine learning são utilizados para detetar o consumo de memória maiores que corresponda a um padrão de uma fuga de memória, ao contrário do consumo de memória aumento devido a naturalmente aumento da utilização da aplicação.

## <a name="does-my-app-definitely-have-a-problem"></a>A minha aplicação sem dúvida tem um problema?
Não, uma notificação não significa que a aplicação sem dúvida tiver um problema. Apesar de padrões de fuga de memória normalmente indicam um problema de aplicação, estes padrões ser típicos para o processo específico, ou pode ter uma justificação de negócio natural e podem ser ignorados.

## <a name="how-do-i-fix-it"></a>Como corrigi-lo?
As notificações incluem informações de diagnóstico para suportar o processo de análise de diagnóstico:
1. **Triagem.** A notificação mostra a aumentar a quantidade de memória (em GB) e o intervalo de tempo em que aumentou a memória. Isto pode ajudar a atribuir uma prioridade para o problema.
2. **Âmbito.** O número de máquinas baixos apresentados o padrão de fuga de memória? O número de exceções foram acionadas durante a potencial fuga de memória? Estas informações podem ser obtidas a partir da notificação.
3. **Diagnosticar.** A deteção contém o padrão de fuga de memória, que mostra o consumo de memória do processo ao longo do tempo. Também pode utilizar os itens relacionados e relatórios de ligação para suportar as informações para ajudá-lo mais diagnosticar o problema.
