---
title: Configurar origens de dados no Log Analytics do Azure | Microsoft Docs
description: "Origens de dados definem os dados que recolhe de análise de registos de agentes e outro ligado origens.  Este artigo descreve o conceito de como Log Analytics utiliza origens de dados, explica os detalhes sobre como configurá-las e fornece um resumo das origens de dados diferentes disponíveis."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2017
ms.author: bwren
ms.openlocfilehash: 4237df0934d6191b77ff82c86a66585e72191ac9
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="data-sources-in-log-analytics"></a>Origens de dados de análise de registos
Análise de registos recolhe dados do seu origens ligadas e armazena-os na sua área de trabalho de análise de registos.  Os dados que são recolhidos de cada um são definidos pelas origens de dados que configurar.  Dados de análise de registos são armazenados como um conjunto de registos.  Cada origem de dados cria registos de um determinado tipo, com cada tipo de ter o seu próprio conjunto de propriedades.

![Recolha de dados de análise de registos](./media/log-analytics-data-sources/overview.png)

Origens de dados são diferentes [soluções de gestão](log-analytics-add-solutions.md), que também recolhe dados de origens ligadas e criar registos na análise de registos.  Para além de recolher dados, as soluções incluem, geralmente, as pesquisas de registo e vistas para ajudar a analisar a operação de uma determinada aplicação ou serviço.


## <a name="summary-of-data-sources"></a>Resumo das origens de dados
As origens de dados que estão atualmente disponíveis no Log Analytics estão listadas na seguinte tabela.  Cada um tem uma ligação para um artigo separado que fornece detalhes para essa origem de dados.

| Origem de Dados | Tipo de evento | Descrição |
|:--- |:--- |:--- |
| [Registos personalizados](log-analytics-data-sources-custom-logs.md) |\<LogName\>_CL |Ficheiros de texto em agentes Windows ou Linux que contém informações de registo. |
| [Registos de eventos do Windows](log-analytics-data-sources-windows-events.md) |Evento |Eventos recolhidos a partir de computadores de Windows de início de sessão de evento. |
| [Contadores de desempenho do Windows](log-analytics-data-sources-performance-counters.md) |Desempenho |Contadores de desempenho recolhidos a partir de computadores Windows. |
| [Contadores de desempenho do Linux](log-analytics-data-sources-performance-counters.md) |Desempenho |Contadores de desempenho recolhidos a partir de computadores com Linux. |
| [Registos do IIS](log-analytics-data-sources-iis-logs.md) |W3CIISLog |Os registos de serviços de informação Internet no formato de W3C. |
| [Syslog](log-analytics-data-sources-syslog.md) |Syslog |Eventos syslog nos computadores Windows ou Linux. |

## <a name="configuring-data-sources"></a>Configurar origens de dados
Configurar origens de dados a partir de **dados** menu na análise de registos **definições avançadas**.  Qualquer configuração é transmitida para todas as origens ligadas na sua área de trabalho.  Atualmente não é possível excluir quaisquer agentes desta configuração.

![Configurar os eventos do Windows](./media/log-analytics-data-sources/configure-events.png)

1. No portal do Azure, selecione **Log Analytics** > sua área de trabalho > **definições avançadas**.
2. Selecione **dados**.
3. Clique na origem de dados que pretende configurar.
4. Siga a ligação para a documentação para cada origem de dados na tabela acima para obter detalhes sobre a respetiva configuração.


## <a name="data-collection"></a>Recolha de dados
Configurações de origem de dados são entregues aos agentes que estão ligados diretamente ao Log Analytics dentro de alguns minutos.  Os dados especificados são recolhidos do agente e entregues diretamente ao Log Analytics intervalos específicos para cada origem de dados.  Consulte a documentação para cada origem de dados para estas informações específicas.

Para agentes do System Center Operations Manager num grupo de gestão ligado, configurações de origem de dados são convertidas em pacotes de gestão e entregue ao grupo de gestão a cada 5 minutos, por predefinição.  O agente transfere o pacote de gestão como qualquer outro e recolhe os dados especificados. Consoante a origem de dados, os dados serão que um enviada para um servidor de gestão que reencaminha os dados para a análise de registos ou o agente irá enviar os dados para análise de registos sem passar o servidor de gestão. Consulte [detalhes de recolha de dados](log-analytics-add-solutions.md#data-collection-details) para obter mais detalhes.  Pode ler sobre os detalhes de ligação do Operations Manager e análise de registos e modificar a frequência de que a configuração é entregue no [configurar a integração com o System Center Operations Manager](log-analytics-om-agents.md).

Se o agente não consegue ligar ao Log Analytics ou o Operations Manager, continua a recolher dados que irá fornecer quando estabelece uma ligação.  Dados podem ser perdidos se a quantidade de dados atinge o tamanho máximo da cache do cliente ou se o agente não é possível estabelecer uma ligação dentro de 24 horas.

## <a name="log-analytics-records"></a>Registos do Log Analytics
Todos os dados recolhidos pela análise de registos é armazenado na área de trabalho como registos.  Registos recolhidos por origens de dados diferentes terão o seu próprio conjunto de propriedades e identificados pelo respetivo **tipo** propriedade.  Consulte a documentação para cada origem de dados e a solução para obter detalhes sobre cada tipo de registo.

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [soluções](log-analytics-add-solutions.md) que adicionar funcionalidades à análise de registos e também recolher dados para a área de trabalho.
* Saiba mais sobre [pesquisas de registo](log-analytics-log-searches.md) para analisar os dados recolhidos a partir de origens de dados e soluções.  
* Configurar [alertas](log-analytics-alerts.md) para proativamente notificá-lo de dados críticos recolhidos a partir de origens de dados e soluções.
