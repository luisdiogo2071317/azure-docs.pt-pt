---
title: Configurar origens de dados do Azure Log Analytics | Documentos da Microsoft
description: Origens de dados definem os dados que recolhe do Log Analytics por agentes e outro ligado origens.  Este artigo descreve o conceito de como o Log Analytics utiliza origens de dados, explica os detalhes de como configurá-las e fornece um resumo das origens de dados diferentes disponíveis.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 071e4694bc3e2a8dc70a199fe7d6021332693b88
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613873"
---
# <a name="data-sources-in-log-analytics"></a>Origens de dados no Log Analytics
Log Analytics recolhe dados a partir de origens ligadas e armazena-os em sua área de trabalho do Log Analytics.  Os dados que são recolhidos de cada um são definidos pelas origens de dados que configurou.  Dados do Log Analytics são armazenados como um conjunto de registos.  Cada origem de dados cria registos de um determinado tipo com cada tipo de ter seu próprio conjunto de propriedades.

![Recolha de dados de análise de registos](./media/log-analytics-data-sources/overview.png)

Origens de dados são diferentes [soluções de gestão](../monitoring/monitoring-solutions.md), que também recolher dados de origens ligadas e criar registos no Log Analytics.  Além de recolher dados, soluções incluem, geralmente, pesquisas de registos e vistas para ajudar a analisar o funcionamento de uma determinada aplicação ou serviço.


## <a name="summary-of-data-sources"></a>Resumo das origens de dados
A tabela seguinte lista as origens de dados que estão atualmente disponíveis no Log Analytics.  Cada um tem uma ligação para um artigo separado, fornecendo detalhes para essa origem de dados.   Também fornece informações no seu método e a frequência de recolha de dados para o Log Analytics.  Pode utilizar as informações neste artigo, para identificar as diferentes soluções disponíveis e para compreender os requisitos de fluxo e a ligação de dados para soluções de gestão diferentes. Para obter explicações das colunas, consulte [detalhes de recolha de dados para soluções de gestão no Azure](../monitoring/monitoring-solutions-inventory.md).


| Origem de dados | Plataforma | Agente de monitorização da Microsoft | Agente do Operations Manager | Storage do Azure | Gestor de operações necessárias? | Dados de agente do Operations Manager enviados por grupo de gestão | Frequência da recolha |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Registos personalizados](log-analytics-data-sources-custom-logs.md) | Windows |&#8226; |  | |  |  | na chegada |
| [Registos personalizados](log-analytics-data-sources-custom-logs.md) | Linux   |&#8226; |  | |  |  | na chegada |
| [Registos do IIS](log-analytics-data-sources-iis-logs.md) | Windows |&#8226; |&#8226; |&#8226; |  |  |depende da definição de Rollover de ficheiro de registo |
| [Contadores de desempenho](log-analytics-data-sources-performance-counters.md) | Windows |&#8226; |&#8226; |  |  |  |mínimo de 10 segundos, conforme agendado |
| [Contadores de desempenho](log-analytics-data-sources-performance-counters.md) | Linux |&#8226; |  |  |  |  |mínimo de 10 segundos, conforme agendado |
| [Syslog](log-analytics-data-sources-syslog.md) | Linux |&#8226; |  |  |  |  |do armazenamento do Azure: 10 minutos; de agente: na chegada |
| [Registos de eventos do Windows](log-analytics-data-sources-windows-events.md) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | na chegada |


## <a name="configuring-data-sources"></a>Configurar origens de dados
Configurar origens de dados a partir da **dados** menu no Log Analytics **definições avançadas**.  Qualquer configuração é entregue para todas as origens ligadas na sua área de trabalho.  Atualmente não é possível excluir quaisquer agentes desta configuração.

![Configurar eventos do Windows](./media/log-analytics-data-sources/configure-events.png)

1. No portal do Azure, selecione **do Log Analytics** > sua área de trabalho > **definições avançadas**.
2. Selecione **dados**.
3. Clique na origem de dados que pretende configurar.
4. Siga a ligação para a documentação para cada origem de dados na tabela acima para obter detalhes sobre a configuração.


## <a name="data-collection"></a>Recolha de dados
Configurações de origem de dados são entregues aos agentes que estão ligadas diretamente ao Log Analytics dentro de alguns minutos.  Os dados especificados são recolhidos a partir do agente e fornecidos diretamente ao Log Analytics em intervalos específicos para cada origem de dados.  Consulte a documentação para cada origem de dados para estas especificações.

Para agentes do System Center Operations Manager num grupo de gestão ligado, configurações de origem de dados são convertidas em pacotes de gestão e entregue ao grupo de gestão a cada 5 minutos por predefinição.  O agente baixa o pacote de gestão como qualquer outro e recolhe os dados especificados. Dependendo da origem de dados, os dados serão que seja enviado para um servidor de gestão que encaminha os dados para o Log Analytics ou o agente irá enviar os dados para o Log Analytics sem passar pelo servidor de gestão. Ver [detalhes de recolha de dados para soluções de gestão no Azure](../monitoring/monitoring-solutions-inventory.md) para obter detalhes.  Pode ler sobre os detalhes da conexão do Operations Manager e o Log Analytics e modificar a frequência que a configuração seja entregue no [configurar a integração com o System Center Operations Manager](log-analytics-om-agents.md).

Se o agente não consegue ligar ao Log Analytics ou o Operations Manager, irá continuar a recolher dados que irá proporcionar quando estabelece uma ligação.  Dados podem ser perdidos se a quantidade de dados atinge o tamanho máximo da cache do cliente, ou se o agente não conseguir estabelecer uma ligação dentro de 24 horas.

## <a name="log-analytics-records"></a>Registos do Log Analytics
Todos os dados recolhidos pelo Log Analytics é armazenado na área de trabalho como registros.  Registos recolhidos por diferentes origens de dados terá seu próprio conjunto de propriedades e ser identificados pelo respetivo **tipo** propriedade.  Consulte a documentação para cada origem de dados e a solução para obter detalhes sobre cada tipo de registo.

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [soluções](../monitoring/monitoring-solutions.md) que acrescentam funcionalidades ao Log Analytics e também, recolher dados para a área de trabalho.
* Saiba mais sobre [pesquisas de registos](log-analytics-queries.md) para analisar os dados recolhidos a partir de origens de dados e soluções.  
* Configurar [alertas](../monitoring-and-diagnostics/monitoring-overview-alerts.md) para ser notificado proativamente dos dados vitais recolhidos a partir de origens de dados e soluções.
