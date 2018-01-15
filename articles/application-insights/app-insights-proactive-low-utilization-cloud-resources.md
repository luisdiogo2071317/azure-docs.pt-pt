---
title: "Smart deteção - utilização baixa de recursos de nuvem detetadas pelo Azure Application Insights | Microsoft Docs"
description: "Monitorizar aplicações com o Azure Application Insights para utilização baixa de recursos de nuvem."
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
ms.openlocfilehash: 8382f6047ae222a01cc0e8d6ca9dcf5593d0dff6
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="low-utilization-of-cloud-resources-preview"></a>Baixa utilização dos recursos de nuvem (pré-visualização)

Application Insights automaticamente analisa o consumo de CPU de cada instância de função na sua aplicação e Deteta instâncias com uma utilização da CPU baixa. Esta deteção permite-lhe diminuir os recursos do Azure e reduzir os custos, ao reduzir o número de instâncias de função que utiliza a cada função, ou ao reduzir o número de funções.

Esta funcionalidade não necessita de nenhuma configuração especial, diferente de [configurar os contadores de desempenho](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) para a sua aplicação. Está ativa quando a sua aplicação gerar telemetria de contador de desempenho suficiente da CPU (% de tempo do processador).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Quando receber este tipo de notificação de deteção inteligente?
Uma notificação típica ocorre quando muitas das suas instâncias de função da Web/trabalho apresentem um baixa utilização da CPU.

## <a name="does-my-app-definitely-consume-too-many-resources"></a>A minha aplicação sem dúvida consumir demasiados recursos?
Não, uma notificação não significa que a aplicação sem dúvida consome demasiados recursos. Embora tais padrões de utilização da CPU baixa normalmente indicam que o consumo de recursos pode ser diminuído, este comportamento pode ser típico para a sua função específica, ou pode ter uma justificação de negócio natural e pode ser ignorado. Por exemplo, poderia ser que várias instâncias são necessárias para outros recursos, como memória/rede e não da CPU.

## <a name="how-do-i-fix-it"></a>Como corrigi-lo?
As notificações incluem informações de diagnóstico para suportar o processo de diagnóstico:
1. **Triagem.** A notificação mostra as funções na sua aplicação que apresentem baixa utilização da CPU. Isto pode ajudar a atribuir uma prioridade para o problema.
2. **Âmbito.** Quantas funções baixos apresentados baixa utilização da CPU e quantas instâncias de cada função utilizam da CPU baixa? Estas informações podem ser obtidas a partir da notificação.
3. **Diagnosticar.** A deteção contém a percentagem de CPU utilizada, que mostra a utilização da CPU de cada instância ao longo do tempo. Também pode utilizar o itens relacionados e os relatórios de ligação para informações, tais como percentiles da utilização da CPU, de suporte para o ajudar a diagnosticar melhor o problema.
