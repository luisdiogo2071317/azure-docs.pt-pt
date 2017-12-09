---
title: "Smart deteção - aumento anormal no volume de exceção, no Azure Application Insights | Microsoft Docs"
description: "Monitorizar exceções da aplicação com o Azure Application Insights para padrões invulgares no volume de exceção."
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
ms.date: 12/08/2017
ms.author: mbullwin
ms.openlocfilehash: 8030f3331a03170bb265c417a57725544bdc7d3f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/09/2017
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Aumento anormal no volume de exceção (pré-visualização)

Application Insights automaticamente analisa as exceções na sua aplicação e pode avisá-lo sobre os padrões invulgares na sua telemetria de exceção.

Esta funcionalidade não necessita de nenhuma configuração especial, diferente de [configurar relatórios de exceção](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-exceptions#set-up-exception-reporting) para a sua aplicação. Está ativa quando a sua aplicação gerar telemetria suficiente exceção.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Quando receber este tipo de notificação de deteção inteligente?
Poderá obter este tipo de notificação se a aplicação esteja a apresentar um aumento anormal do número de exceções de um tipo específico durante um dia, em comparação com uma linha de base calculada ao longo de últimos sete dias.
Algoritmos de Machine learning estão a ser utilizados para detetar o aumento súbito do número de exceção, ao tendo em conta um crescimento natural na utilização da sua aplicação.

## <a name="does-my-app-definitely-have-a-problem"></a>A minha aplicação sem dúvida tem um problema?
Não, uma notificação não significa que a aplicação sem dúvida tiver um problema. Embora um número excessivo de exceções normalmente indica um problema de aplicação, estas exceções podem ser benignas e processadas corretamente pela sua aplicação.

## <a name="how-do-i-fix-it"></a>Como corrigi-lo?
As notificações incluem informações de diagnóstico para suportar o processo de diagnóstico:
1. **Triagem.** A notificação mostra quantos utilizadores ou o número de pedidos que é afetado. Isto pode ajudar a atribuir uma prioridade para o problema.
2. **Âmbito.** É o problema que afetam a todo o tráfego ou apenas a alguns operação? Estas informações podem ser obtidas a partir da notificação.
3. **Diagnosticar.** A deteção contém informações sobre o método a partir da qual a exceção foi acionada, bem como o tipo de exceção. Também pode utilizar os itens relacionados e relatórios de ligação para suportar as informações para ajudá-lo mais diagnosticar o problema.