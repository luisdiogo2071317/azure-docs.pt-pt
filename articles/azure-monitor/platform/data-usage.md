---
title: Analisar a utilização de dados do Log Analytics | Microsoft Docs
description: Utilize o dashboard “Utilização e custo estimado” no Log Analytics para avaliar a quantidade de dados que são enviados para o Log Analytics e identificar o que poderá provocar aumentos não previstos.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/11/2018
ms.author: magoedte
ms.openlocfilehash: af41e2af87afbbeb8629d07508d4e5244351a4df
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53277357"
---
# <a name="analyze-data-usage-in-log-analytics"></a>Analisar a utilização de dados do Log Analytics

> [!NOTE]
> Este artigo descreve como analisar a utilização de dados do Log Analytics.  Veja os artigos seguintes para obter informações relacionadas.
> - [Gerir os custos ao controlar o volume de dados e a retenção do Log Analytics](manage-cost-storage.md) descreve como controlar os custos ao alterar o período de retenção de dados.
> - [Monitorizar a utilização e custos estimados](../../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) descreve como ver a utilização e custos estimados no Azure de várias funcionalidades para diferentes modelos de preços de monitorização. Também descreve como alterar o modelo de preços.

## <a name="understand-usage"></a>Compreender a utilização

Uso **utilização do Log Analytics e os custos estimados** para rever e analisar a utilização de dados. Mostra a quantidade de dados é recolhido por cada solução, a quantidade de dados está a ser retido e uma estimativa dos seus custos com base na quantidade de dados ingeridos e qualquer retenção adicional que ultrapassem o montante incluído.

![Utilização e custos estimados](media/data-usage/usage-estimated-cost-dashboard-01.png)<br>

Para explorar os seus dados em mais detalhes, clique no ícone na parte superior direita do qualquer um dos gráficos no **utilização e custos estimados** página. Agora pode trabalhar com esta consulta para explorar mais detalhes da sua utilização.  

![Ver registos](media/data-usage/logs.png)<br>

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Resolver o motivo pelo qual a utilização é superior ao esperado
A utilização superior deve-se a um ou a ambos os motivos abaixo:
- Estão a ser enviados para o Log Analytics mais dados do que o esperado
- Alguns nós ou mais nós que o envio de dados esperado para o Log Analytics estão a enviar mais dados que o normal

Vamos dar uma olhada em como podemos pode saber mais sobre ambos estes causas. 

> [!NOTE]
> Alguns dos campos do tipo de dados de utilização, enquanto ainda está no esquema, foram preteridos e já não são preenchidos a seus valores. Estes são **computador** , bem como campos relacionados com a ingestão (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**,  **BatchesCapped** e **AverageProcessingTimeMs**.
> Veja abaixo para a nova forma de consultar a quantidade de dados ingeridos por computador. 

### <a name="data-volume"></a>Volume de dados 
Sobre o **utilização e custos estimados** página, o *ingestão de dados por solução* gráfico mostra o volume total de dados enviados e a quantidade está a ser enviado por cada solução. Isto permite-lhe determinar as tendências, como se a utilização de dados global (ou a utilização por uma solução específica) está a crescer, permanece estável ou diminui. É a consulta usada para gerar este

`Usage| where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart`

Tenha em atenção que a cláusula "onde IsBillable = true" filtra tipos de dados de algumas soluções para o qual não é cobrado ingestão. 

Pode explorar mais a ver as tendências de dados para tipos de dados específicos, por exemplo, se quiser estudar os dados devido a registos do IIS:

`Usage| where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| where DataType == "W3CIISLog"
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart`

### <a name="nodes-sending-data"></a>Nós a enviar dados

Para compreender o número de computadores (nós), dados de relatórios no mês passado, utilize

`Heartbeat | where TimeGenerated > startofday(ago(31d))
| summarize dcount(ComputerIP) by bin(TimeGenerated, 1d)    
| render timechart`

Para ver os **tamanho** de eventos a cobrar ingeridos por computador, utilize o `_BilledSize` propriedade que fornece o tamanho em bytes:

`union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by  Computer | sort by Bytes nulls last `

Utilize estas consultas com moderação, como análises em dados dados typres são dispendiosas. Esta consulta substitui o método antigo de consulta isso com o tipo de dados de utilização. 

Para ver os **contagem** de eventos ingeridos por computador, utilize

`union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last`

Para ver a contagem de eventos a cobrar ingeridos por computador, utilize 

`union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last`

Se quiser ver contagens para tipos de dados cobrar estão a enviar dados para um computador específico, utilize:

`union withsource = tt *
| where Computer == "*computer name*"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last `

Para saber mais sobre a origem de dados para um tipo de dados específico, aqui estão algumas consultas de exemplo úteis:

+ Solução de **Segurança**
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ Solução de **Gestão de Registos**
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ Tipo de dados de **Desempenho**
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ Tipo de dados de **Evento**
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ Tipo de dados de **Syslog**
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ Tipo de dados de **AzureDiagnostics**
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

### <a name="tips-for-reducing-data-volume"></a>Dicas para reduzir o volume de dados

Algumas sugestões para reduzir o volume de registos recolhidos incluem:

| Origem do volume de dados elevado | Como reduzir o volume de dados |
| -------------------------- | ------------------------- |
| Eventos de segurança            | Selecione [eventos de segurança comuns ou mínimos](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) <br> Altere a política de auditoria de segurança para recolher apenas os eventos necessários. Em particular, reveja a necessidade de recolher eventos para <br> - [auditar a plataforma de filtragem](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [auditar o registo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [auditar o sistema de ficheiros](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [auditar o objeto de kernel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [auditar a manipulação de identificadores](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> -Auditar o armazenamento amovível |
| Contadores de desempenho       | Altere a [configuração do contador de desempenho](data-sources-performance-counters.md) para: <br> - Reduzir a frequência da recolha <br> - Reduzir o número de contadores de desempenho |
| Registos de eventos                 | Altere a [configuração do registo de eventos](data-sources-windows-events.md) para: <br> - Reduzir o número de registos de eventos recolhidos <br> - Recolher apenas níveis de eventos necessários. Por exemplo, não recolher eventos de nível *Informação* |
| Syslog                     | Altere a [configuração do syslog](data-sources-syslog.md) para: <br> - Reduzir o número de instalações recolhidas <br> - Recolher apenas níveis de eventos necessários. Por exemplo, não recolher eventos de nível *Informação* e *Depuração* |
| AzureDiagnostics           | Alterar a coleção de registo de recursos para: <br> - Reduzir o número de registos de envio de recursos do Log Analytics <br> - Recolher apenas registos necessários |
| Dados de solução de computadores que não precisam da solução | Utilize a [segmentação de soluções](../../azure-monitor/insights/solution-targeting.md) para recolher dados apenas de grupos de computadores necessários. |

### <a name="getting-node-counts"></a>Ao obter contagens de nó 

Se estiver a utilizar "Por nó (OMS)" escalão de preço, em seguida, é cobrado com base no número de nós e soluções utilizar, o número de informações e nós de análise que lhe está a ser cobrada serão apresentados na tabela no **utilização e custos estimados**página.  

Para ver o número de nós de segurança distintas, pode utilizar a consulta:

`union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count`

Para ver o número de nós de automatização distintos, utilize a consulta:

` ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc`

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Criar um alerta quando a recolha de dados for superior ao esperado
Esta secção descreve como criar um alerta se:
- O volume de dados exceder uma determinada quantidade.
- Se previr que o volume de dados vai exceder uma determinada quantidade.

Os Alertas do Azure suportam [registar alertas](../../azure-monitor/platform/alerts-unified-log.md) que utilizam consultas de pesquisa. 

A consulta seguinte tem um resultado quando são recolhidos mais de 100 GB de dados nas últimas 24 horas:

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`

A consulta seguinte utiliza uma fórmula simples para prever quando é que vão ser enviados mais de 100 GB de dados num dia: 

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`

Para alertar relativamente a volumes de dados diferentes, altere o 100 nas consultas para o número de GB para o qual quer receber o alerta.

Utilize os passos descritos em [create a new log alert](../../azure-monitor/platform/alerts-metric.md) (criar um novo alerta de registo) para ser notificado de quando a recolha de dados for superior ao esperado.

Quando criar o alerta para a primeira consulta – quando existem mais de 100 GB de dados em 24 horas –, defina:  

- **Definir condição de alerta** especifique a sua área de trabalho do Log Analytics como o destino de recursos.
- **Critérios de alerta** especifique o seguinte:
   - **Nome do Sinal** selecione **Pesquisa de registos personalizada**
   - A **consulta de pesquisa** como `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`
   - A **Lógica de alerta** é **Baseada no** *número de resultados* e a **Condição** é *Maior do que* um **Limiar** de *0*
   - **Período de tempo** de *1440* minutos e **Frequência de alertas** a cada *60* minutos, uma vez que os dados de utilização são atualizados apenas uma vez por hora.
- **Definir detalhes do alerta** especifique o seguinte:
   - O **nome** como *Volume de dados maior do que 100 GB em 24 horas*
   - A **gravidade** como *Aviso*

Especifique um existente ou crie um novo [Grupo de Ação](../../azure-monitor/platform/action-groups.md), para que quando o alerta de registo corresponda aos critérios, seja notificado.

Quando criar o alerta para a segunda consulta – quando se previr que vai haver mais de 100 GB de dados em 24 horas –, defina:

- **Definir condição de alerta** especifique a sua área de trabalho do Log Analytics como o destino de recursos.
- **Critérios de alerta** especifique o seguinte:
   - **Nome do Sinal** selecione **Pesquisa de registos personalizada**
   - A **consulta de pesquisa** como `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`
   - A **Lógica de alerta** é **Baseada no** *número de resultados* e a **Condição** é *Maior do que* um **Limiar** de *0*
   - **Período de tempo** de *180* minutos e **Frequência de alertas** a cada *60* minutos, uma vez que os dados de utilização são atualizados apenas uma vez por hora.
- **Definir detalhes do alerta** especifique o seguinte:
   - O **nome** como *Previsto volume de dados maior do que 100 GB em 24 horas*
   - A **gravidade** como *Aviso*

Especifique um existente ou crie um novo [Grupo de Ação](../../azure-monitor/platform/action-groups.md), para que quando o alerta de registo corresponda aos critérios, seja notificado.

Quando receber um alerta, utilize os passos da secção seguinte para resolver o motivo pelo qual a utilização é superior ao esperado.

## <a name="next-steps"></a>Passos Seguintes
* Veja [Pesquisas de registos no Log Analytics](../log-query/log-query-overview.md) para aprender a utilizar a linguagem de pesquisa. Pode utilizar as consultas de pesquisa para executar análises adicionais aos dados de utilização.
* Utilize os passos descritos em [create a new log alert](../../azure-monitor/platform/alerts-metric.md) (criar um novo alerta de registo) para ser notificado de quando um critério de pesquisa for cumprido.
* Utilize a [segmentação de soluções](../insights/solution-targeting.md) para recolher dados apenas de grupos de computadores necessários.
* Para configurar uma política de recolha de eventos de segurança eficaz, veja [Política de filtragem do Centro de Segurança do Azure](../../security-center/security-center-enable-data-collection.md).
* Altere a [configuração do contador de desempenho](data-sources-performance-counters.md).
* Para modificar as definições da recolha de eventos, veja [configuração do registo de eventos](data-sources-windows-events.md).
* Para modificar as definições da recolha do syslog, veja [configuração do syslog](data-sources-syslog.md).
