---
title: Descrição geral das métricas no Azure Monitor
description: Saiba como personalizar a gráficos de monitorização no Azure.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/06/2017
ms.author: robb
ms.component: metrics
ms.openlocfilehash: 44daf6461a062e75435ec6f70fbc3cf10327e799
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213049"
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Descrição geral das métricas no Microsoft Azure
Todos os serviços Azure controlam métricas-chave que permitem-lhe monitorizar o estado de funcionamento, desempenho, disponibilidade e utilização dos seus serviços. Pode ver estas métricas no portal do Azure, e também pode utilizar o [REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) ou [SDK do .NET](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) para aceder ao conjunto completo de métricas programaticamente.

Para alguns serviços, terá de ativar os diagnósticos para ver todas as métricas. Para outros, como as máquinas virtuais, obterá um conjunto básico de métricas, mas precisa ativar o completo, definir métricas de elevada frequência. Ver [ativar a monitorização e diagnóstico](insights-how-to-use-diagnostics.md) para saber mais.

## <a name="using-monitoring-charts"></a>Utilizar gráficos de monitorização
Pode gráfico qualquer uma das métricas-los a qualquer período de tempo que escolher.

1. Na [Portal do Azure](https://portal.azure.com/), clique em **procurar**, e, em seguida, um recurso tiver interesse em monitorização.
2. O **monitorização** secção contém as métricas mais importantes para cada recurso do Azure. Por exemplo, uma aplicação web possui **pedidos e erros**, em que como uma máquina virtual teria **percentagem de CPU** e **leitura e escrita de disco**: ![lente de monitorização](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)
3. Clicar em qualquer gráfico mostrará a o **métrica** painel. No painel, além de gráfico, é uma tabela que mostra as agregações das métricas (como média, mínima e máxima, ao longo do intervalo de tempo que escolheu). Abaixo disso são as regras de alerta para o recurso.
    ![Painel métricas](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)
4. Para personalizar as linhas que são apresentadas, clique nas **editar** botão no gráfico, em alternativa, o **editar gráfico** comando no painel de métrico.
5. No painel Editar consulta, pode fazer três coisas:
   
   * Alterar o intervalo de tempo
   * Mude a aparência entre barra e a linha
   * Escolher diferentes metics ![Editar consulta](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)
6. Alterar o intervalo de tempo é tão fácil como selecionar um intervalo diferente (por exemplo, **última hora**) e clicando em **guardar** na parte inferior do painel. Também pode escolher **personalizado**, que permite-lhe escolher qualquer período de tempo durante as últimas duas semanas. Por exemplo, pode ver o todos duas semanas, ou, apenas 1 hora de ontem. Escreva na caixa de texto para introduzir uma hora diferente.
    ![Intervalo de tempo personalizado](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)
7. Abaixo o intervalo de tempo, chan, escolha um número qualquer de métricas para mostrar no gráfico.
8. Ao clicar em guardar as alterações serão guardadas para esse recurso em particular. Por exemplo, se tem duas máquinas virtuais e alterar um gráfico em um, ele não irá afetar a outra.

## <a name="creating-side-by-side-charts"></a>Criando gráficos de lado a lado
Com a personalização eficiente no portal do pode adicionar quantos gráficos que quiser.

1. Na **...**  menu na parte superior do painel, clique em **adicionar mosaicos**:  
    ![Menu adicionar](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Em seguida, pode selecionar um gráfico do **galeria** no lado direito da tela: ![Galeria](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Se não vir a métrica desejar, poderá adicionar uma das métricas predefinidas, e **editar** o gráfico para mostrar a métrica que precisa.

## <a name="monitoring-usage-quotas"></a>Monitorizar quotas de utilização
A maioria das métricas mostram tendências ao longo do tempo, mas alguns dados, como quotas de utilização são informações de ponto no tempo com um limiar.

Também pode ver quotas de utilização no painel de recursos que tenham as quotas:

![Utilização](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Como com a métrica, pode utilizar o [REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) ou [SDK do .NET](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) para acessar programaticamente o conjunto completo de quotas de utilização.

## <a name="next-steps"></a>Passos Seguintes
* [Receber notificações de alerta](insights-receive-alert-notifications.md) sempre que uma métrica ultrapassa um limiar.
* [Ative a monitorização e diagnóstico](insights-how-to-use-diagnostics.md) para recolher métricas de alta frequência detalhadas em seu serviço.
* [Dimensionar automaticamente o número de instâncias](insights-how-to-scale.md) para se certificar de que o seu serviço está disponível e reativo.
* [Monitorizar o desempenho do aplicativo](../application-insights/app-insights-azure-web-apps.md) se quiser compreender exatamente como seu código está sendo executada na cloud.
* Uso [Application Insights para aplicações JavaScript e páginas da web](../application-insights/app-insights-web-track-usage.md) para obter análises de cliente sobre os browsers que visitam as páginas web.
* [Monitorizar a disponibilidade e capacidade de resposta de qualquer página da web](../application-insights/app-insights-monitor-web-app-availability.md) com o Application Insights para que pode descobrir se a sua página está inativo.

