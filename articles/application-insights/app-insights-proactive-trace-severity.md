---
title: "Smart deteção - degradação no rácio de gravidade de rastreio, no Azure Application Insights | Microsoft Docs"
description: "Monitorizar rastreios de aplicações com o Azure Application Insights para padrões invulgares na telemetria de rastreio."
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
ms.date: 11/27/2017
ms.author: mbullwin
ms.openlocfilehash: 29ed195dadb7230df425d6d981a0a482e09ee79f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/09/2017
---
# <a name="degradation-in-trace-severity-ratio-preview"></a>Degradação no rácio de gravidade de rastreio (pré-visualização)

Os rastreios amplamente são utilizados em aplicações, como podem ajudar a informar o bloco do que acontece em segundo plano. Se as coisas correrem mal, rastreios fornecem visibilidade fundamental para a sequência de eventos à esquerda para o estado indesejado. Enquanto os rastreios são geralmente não estruturados, há única coisa que pode ser adquirida concretely dos mesmos – respetivo nível de gravidade. Estado de repouso de uma aplicação, iremos seria de esperar o rácio entre rastreios "boas" (*informações* e *verboso*) e rastreios "incorretos" (*aviso*, *erro* e *crítico*) permaneça estável. É do pressuposto de que "incorretos" rastreios podem acontecer regularmente uma determinada extensão devido a qualquer número de motivos (rede transitória problemas para a instância). Mas, quando um problema real, começa a crescer normalmente manifestos como um aumento da proporção relativo de rastreios de "boas" rastreios "incorretos" vs. Application Insights inteligente deteção automaticamente analisa os rastreios registados pela sua aplicação e pode avisá-lo sobre os padrões invulgares na gravidade da sua telemetria de rastreio.

Esta funcionalidade não necessita de nenhuma configuração especial, além de configurar o registo de rastreio para a sua aplicação (consulte como configurar um serviço de escuta de registo de rastreio para [.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-trace-logs) ou [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-trace-logs)). Está ativa quando a sua aplicação gerar telemetria suficiente exceção.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Quando receber este tipo de notificação de deteção inteligente?
Poderá obter este tipo de notificação se o rácio entre rastreios "boas" (rastreios registados com um nível de *informações* ou *verboso*) e rastreios "incorretos" (rastreios registados com um nível de *aviso*, * Erro, ou *Fatal*) é degradar num dia específico, em comparação comparado uma linha de base calculada ao longo de últimos sete dias.

## <a name="does-my-app-definitely-have-a-problem"></a>A minha aplicação sem dúvida tem um problema?
Não, uma notificação não significa que a aplicação sem dúvida tiver um problema. Embora uma degradação no rácio entre "boas" e "incorretos" rastreios pode indicar um problema de aplicação, esta alteração rácio em pode ser benignas. Por exemplo, o aumento pode ser devido a um novo fluxo na aplicação emitir mais rastreios "incorretos" que fluxos existentes).

## <a name="how-do-i-fix-it"></a>Como corrigi-lo?
As notificações incluem informações de diagnóstico para suportar o processo de diagnóstico:
1. **Triagem.** A notificação mostra-lhe como muitas operações são afetadas. Isto pode ajudar a atribuir uma prioridade para o problema.
2. **Âmbito.** É o problema que afetam a todo o tráfego ou apenas a alguns operação? Estas informações podem ser obtidas a partir da notificação.
3. **Diagnosticar.** Pode utilizar os itens relacionados e relatórios de ligação para suportar as informações para ajudá-lo mais diagnosticar o problema.


