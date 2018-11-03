---
title: Recolher e analisar registos de eventos do Windows no Azure Log Analytics | Documentos da Microsoft
description: Registos de eventos do Windows são uma das origens de dados mais comuns utilizadas pelo Log Analytics.  Este artigo descreve como configurar a recolha de registos de eventos do Windows e os detalhes dos registos que criaram na área de trabalho do Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: ee52f564-995b-450f-a6ba-0d7b1dac3f32
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2017
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 0285b9208247aa84f56d127e734e8a5e87048ac6
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961369"
---
# <a name="windows-event-log-data-sources-in-log-analytics"></a>Origens de dados de registo de eventos Windows no Log Analytics
Registos de eventos do Windows são uma das mais comuns [origens de dados](log-analytics-data-sources.md) para a recolha de dados através de agentes do Windows, uma vez que muitos aplicativos escrevem no registo de eventos do Windows.  Pode recolher eventos de registos standard como sistema e da aplicação, além de especificar quaisquer registos personalizados criados por aplicativos que tem de monitorizar.

![Eventos do Windows](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Os logs de eventos do Windows configuração
Configurar registos de eventos do Windows a partir do [menu de dados nas definições do Log Analytics](log-analytics-data-sources.md#configuring-data-sources).

Apenas do log Analytics recolhe os eventos dos registos de eventos do Windows que são especificados nas definições.  Pode adicionar um registo de eventos ao escrever o nome do registo e clicar em **+**.  Para cada registo, apenas os eventos com as gravidades selecionados são recolhidos.  Verifique as gravidades para o log específico que pretende recolher.  Não pode fornecer qualquer critérios adicionais para filtrar eventos.

À medida que escreve o nome de um registo de eventos, o Log Analytics fornece sugestões de nomes de registo de eventos comuns. Se o registo que pretende adicionar não aparecer na lista, ainda pode adicioná-lo ao escrever o nome completo do registo. Pode encontrar o nome completo do registo utilizando o Visualizador de eventos. No Visualizador de eventos, abra a *propriedades* página para o log e copie a cadeia a partir do *nome completo* campo.

![Configurar eventos do Windows](media/log-analytics-data-sources-windows-events/configure.png)

## <a name="data-collection"></a>Recolha de dados
O log Analytics recolhe cada evento que corresponde a uma gravidade selecionada de um registo de eventos monitorado, como o evento é criado.  O agente registra seu lugar em cada registo de eventos recolhidos dos.  Se o agente ficar offline durante um período de tempo, em seguida, do Log Analytics recolhe os eventos de onde pela última vez parou, mesmo que esses eventos foram criados, enquanto o agente estava offline.  Existe um potencial para esses eventos para não ser recolhidos se o registo de eventos encapsula num wrapper com eventos uncollected seja substituídos enquanto o agente estiver offline.

>[!NOTE]
>O log Analytics recolhe os eventos de auditoria criados pelo SQL Server de origem *MSSQLSERVER* com o ID de evento 18453 que contém as palavras-chave - *clássico* ou *sucesso de auditoria* e palavra-chave *0xa0000000000000*.
>

## <a name="windows-event-records-properties"></a>Propriedades de registos de eventos do Windows
Registos de eventos do Windows têm um tipo de **evento** e ter as propriedades na tabela a seguir:

| Propriedade | Descrição |
|:--- |:--- |
| Computador |Nome do computador que o evento foi recolhido a partir de. |
| Se nos |Categoria do evento. |
| EventData |Todos os dados de eventos em formato não processado. |
| eventID |Número do evento. |
| eventLevel |Gravidade do evento no formato numérico. |
| EventLevelName |Gravidade do evento na forma de texto. |
| Registo de eventos |Nome do registo de eventos que o evento foi recolhido a partir de. |
| ParameterXml |Valores de parâmetros de evento no formato XML. |
| ManagementGroupName |Nome do grupo de gestão para agentes do System Center Operations Manager.  Para outros agentes, este valor é AOI-<workspace ID> |
| RenderedDescription |Descrição do evento com valores de parâmetros |
| Origem |Origem do evento. |
| SourceSystem |Tipo do evento foi recolhido a partir do agente. <br> Ligar OpsManager – agente de Windows, direta ou gerido do Operations Manager <br> Linux – todos os agentes do Linux  <br> AzureStorage – diagnósticos do Azure |
| TimeGenerated |Data e hora que do evento foi criado no Windows. |
| Nome de Utilizador |Nome de utilizador da conta que registou o evento. |

## <a name="log-searches-with-windows-events"></a>Pesquisas de registos com eventos do Windows
A tabela seguinte fornece exemplos diferentes de pesquisas de registos que obter registos de eventos do Windows.

| Consulta | Descrição |
|:---|:---|
| Evento |Todos os eventos do Windows. |
| Evento &#124; onde EventLevelName = = "erro" |Todos os eventos do Windows com a gravidade do erro. |
| Evento &#124; resumir contagem () por origem |Eventos de contagem do Windows pela origem. |
| Evento &#124; onde EventLevelName = = "erro" &#124; resumir contagem () por origem |Eventos de erro de contagem do Windows pela origem. |


## <a name="next-steps"></a>Passos Seguintes
* Configurar o Log Analytics para recolher outros [origens de dados](log-analytics-data-sources.md) para análise.
* Saiba mais sobre [pesquisas de registos](log-analytics-log-search.md) para analisar os dados recolhidos a partir de origens de dados e soluções.  
* Uso [campos personalizados](log-analytics-custom-fields.md) para analisar os registos de eventos em campos individuais.
* Configurar [recolha de contadores de desempenho de](log-analytics-data-sources-performance-counters.md) dos seus agentes do Windows.
