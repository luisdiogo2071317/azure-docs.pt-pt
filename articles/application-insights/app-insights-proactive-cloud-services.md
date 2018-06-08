---
title: Alerta sobre problemas nos serviços de nuvem do Azure utilizando a integração de diagnóstico do Azure com o Azure Application Insights | Microsoft Docs
description: Monitor de problemas como falhas de arranque, as falhas e função Reciclagem ciclos nos serviços de nuvem do Azure com o Azure Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: harelbr; mbullwin
ms.openlocfilehash: 18817fd84a86a72d379f96973b71658f2cdf4afd
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34851323"
---
# <a name="alert-on-issues-in-azure-cloud-services-using-the-azure-diagnostics-integration-with-azure-application-insights"></a>Alerta sobre problemas nos serviços de nuvem do Azure utilizando a integração de diagnóstico do Azure com o Azure Application Insights

Neste artigo, vamos descrevem como configurar regras de alertas que monitorizam a existência de problemas como falhas de arranque, as falhas e função Reciclagem ciclos nos serviços em nuvem do Azure (funções web e de trabalho).

O método descrito neste artigo baseia-se no [integração de diagnóstico do Azure com o Application Insights](https://azure.microsoft.com/blog/azure-diagnostics-integration-with-application-insights/)e recentemente lançadas [alertas de registo para o Application Insights](https://azure.microsoft.com/blog/log-alerts-for-application-insights-preview/) capacidade.

## <a name="define-a-base-query"></a>Definir uma consulta de base

Para começar, iremos definir uma consulta de base que obtém os eventos de registo de eventos do Windows a partir do canal do Windows Azure, que são capturados no Application Insights como registos de rastreio.
Estes registos podem ser utilizados para detetar uma variedade de problemas na Cloud Services do Azure, como falhas de arranque, falhas de tempo de execução e reciclagem ciclos.

> [!NOTE]
> A consulta de base abaixo verifica a existência de problemas de uma janela de tempo de 30 minutos e assume uma latência de 10 minutos em ingestão relacionadas os registos de telemetria. Estas predefinições podem ser configuradas como julgar.

```
let window = 30m;
let endTime = ago(10m);
let EventLogs = traces
| where timestamp > endTime - window and timestamp < endTime
| extend channel = tostring(customDimensions.Channel), eventId = tostring(customDimensions.EventId)
| where channel == 'Windows Azure' and isnotempty(eventId)
| where tostring(customDimensions.DeploymentName) !contains 'deployment' // discard records captured from local machines
| project timestamp, channel, eventId, message, cloud_RoleInstance, cloud_RoleName, itemCount;
```

## <a name="check-for-specific-event-ids"></a>Verifique a existência de IDs de evento específico

Após a obtenção de eventos de registo de eventos do Windows, podem ser detetados problemas específicos verificando as respetivas propriedades de ID e a mensagem de evento correspondentes (Consulte exemplos abaixo).
Combine simplesmente a consulta de base acima com uma das consultas abaixo e utilizadas que combinado de consulta quando definir a regra de alerta de registo.

> [!NOTE]
> Nos exemplos abaixo, irá ser detetado um problema se forem encontrados mais do que três eventos durante a janela de tempo analisado. Esta predefinição pode ser configurada para alterar a sensibilidade de regra de alerta.

```
// Detect failures in the OnStart method
EventLogs
| where eventId == '2001'
| where message contains '.OnStart()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures during runtime
EventLogs
| where eventId == '2001'
| where message contains '.Run()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures when running a startup task
EventLogs
| where eventId == '1000'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect recycle loops
EventLogs
| where eventId == '1006'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

## <a name="create-an-alert"></a>Criar um alerta

No menu de navegação no recurso do Application Insights, aceda a **alertas**e, em seguida, selecione **nova regra de alerta**.

![Captura de ecrã de criar regra](./media/app-insights-proactive-cloud-services/001.png)

No **criar regra** janela, sob o **definir a condição de alerta** secção, clique em **adicionar critérios**e, em seguida, selecione **pesquisa de registo personalizado**.

![Captura de ecrã de definir critérios de condição de alerta](./media/app-insights-proactive-cloud-services/002.png)

No **consulta de pesquisa** caixa, cole a consulta combinada que está preparado no passo anterior.

Em seguida, avance para o **limiar** caixa e defina o respetivo valor como 0. Opcionalmente, pode otimizar o **período** e a frequência **campos**.
Clique em **Concluído**.

![Captura de ecrã da consulta de lógica de sinal de configurar](./media/app-insights-proactive-cloud-services/003.png)

Sob o **definir os detalhes do alerta** secção, forneça um **nome** e **Descrição** para a regra de alerta e defina o **gravidade**.
Além disso, certifique-se de que o **ativar regra após a criação** botão está definido como **Sim**.

![Detalhes do alerta de captura de ecrã](./media/app-insights-proactive-cloud-services/004.png)

Sob o **grupo de ação de definir** secção, pode selecionar um existente **grupo ação** ou crie um novo.
Pode optar por grupo de ação contêm várias ações de vários tipos.

![Grupo de ação de captura de ecrã](./media/app-insights-proactive-cloud-services/005.png)

Uma vez que definiu o grupo de ação, confirme as suas alterações e clique em **criar regra de alerta**.

## <a name="next-steps"></a>Próximos Passos

Saiba mais sobre como detetar automaticamente:

[Anomalias falha](app-insights-proactive-failure-diagnostics.md)
[fugas de memória](app-insights-proactive-potential-memory-leak.md)
[anomalias de desempenho](app-insights-proactive-performance-diagnostics.md)

