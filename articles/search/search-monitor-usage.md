---
title: Monitorizar a utilização e estatísticas para um serviço de pesquisa - Azure Search
description: Controlar o tamanho de consumo e o índice de recursos do Azure Search, um serviço de pesquisa de nuvem alojada no Microsoft Azure.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 11/09/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 584d1d8ce3285f9f5fb986c9779d3c403ce13d1b
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314164"
---
# <a name="monitor-an-azure-search-service-in-azure-portal"></a>Monitorizar um serviço de Azure Search no portal do Azure

O Azure Search oferece vários recursos para o controlo de utilização e desempenho dos serviços de pesquisa. Dá-lhe acesso a métricas, registos, estatísticas de índice e capacidades de monitorização expandidas no Power BI. Este artigo descreve como ativar as diferentes estratégias de monitorização e como interpretar os dados resultantes.

## <a name="azure-search-metrics"></a>Métricas de pesquisa do Azure
Métricas dão-lhe perto de visibilidade em tempo real para o serviço de pesquisa e estão disponíveis para cada serviço, com nenhuma configuração adicional. Eles permitem-lhe controlar o desempenho do seu serviço durante 30 dias.

O Azure Search recolhe dados de três métricas diferentes:

* Latência de pesquisa: O serviço de pesquisa necessário para processar consultas de pesquisa, agregadas por cada minuto de tempo.
* Consultas de pesquisa por segundo (QPS): Número de pesquisa agregada de consultas recebidas por segundo, por minuto.
* Percentagem de consultas de pesquisa limitados: Percentagem de consultas de pesquisa que eram limitados, agregados por cada minuto.

![Atividade de captura de ecrã de QPS][1]

### <a name="set-up-alerts"></a>Configurar alertas
A página de detalhes de métrica, pode configurar alertas para acionar uma notificação por e-mail ou uma ação automática quando uma métrica ultrapassa um limiar que definiu.

Para obter mais informações sobre as métricas, verifique a documentação completa sobre o Azure Monitor.  

## <a name="how-to-track-resource-usage"></a>Como controlar a utilização de recursos
Controlar o crescimento de índices e o tamanho do documento pode ajudá-lo a ajuste capacidade de forma proativa antes de atingir o limite superior criada para o seu serviço. Pode fazê-lo no portal ou programaticamente com a API REST.

### <a name="using-the-portal"></a>Utilizar o portal

Para monitorizar a utilização de recursos, veja as contagens e estatísticas para o seu serviço no [portal](https://portal.azure.com).

1. Inicie sessão no [portal](https://portal.azure.com).
2. Abra o dashboard de serviço do seu serviço da Azure Search. Mosaicos para o serviço podem ser encontrados na Home page ou, pode navegar para o serviço de procura na barra de índice.

A secção utilização inclui um medidor que diz a que parte dos recursos disponíveis estão atualmente em utilização. Para obter informações sobre os limites por serviço de armazenamento, índices e documentos, consulte [limites de serviço](search-limits-quotas-capacity.md).

  ![Mosaico utilização][2]

> [!NOTE]
> Captura de tela acima é para o serviço gratuito, que tem um máximo de uma réplica e cada um de partição e pode apenas índices de anfitrião, 3, 10.000 documentos ou 50 MB de dados, o que ocorrer primeiro. Serviços criados num escalão básico ou Standard têm muito maiores limites de serviço. Para obter mais informações sobre como escolher um escalão, consulte [escolher um escalão ou SKU](search-sku-tier.md).
>
>

### <a name="using-the-rest-api"></a>Utilizar a API REST
A API de REST do Azure Search e o SDK do .NET fornecem acesso programático a métricas de serviço.  Se estiver a utilizar [indexadores](https://msdn.microsoft.com/library/azure/dn946891.aspx) para carregar um índice de base de dados do Azure SQL ou do Azure Cosmos DB, uma API adicional está disponível para obter os números que necessita.

* [Obter estatísticas de índice](/rest/api/searchservice/get-index-statistics)
* [Contagem de documentos](/rest/api/searchservice/count-documents)
* [Obter estado do indexador](/rest/api/searchservice/get-indexer-status)

## <a name="how-to-export-logs-and-metrics"></a>Como exportar os registos e métricas

Pode exportar os registos de operações para o seu serviço e os dados não processados para as métricas descritos na secção anterior. Operação de registos permitem que sabe como o serviço está a ser utilizado e pode ser utilizado a partir do Power BI quando dados são copiados para uma conta de armazenamento. O Azure search fornece um pacote de conteúdos do Power BI monitorização para esta finalidade.


### <a name="enabling-monitoring"></a>A ativação da monitorização
Abra o serviço Azure Search no [portal do Azure](http://portal.azure.com) sob a opção de ativar a monitorização.

Escolha os dados que pretende exportar: Os registos, métricas ou ambos. Pode copiá-lo para uma conta de armazenamento, enviá-lo para um hub de eventos ou exportá-lo para o Log Analytics.

![Como ativar a monitorização no portal][3]

Para ativar a utilizar o PowerShell ou a CLI do Azure, consulte a documentação [aqui](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

### <a name="logs-and-metrics-schemas"></a>Esquemas de registos e métricas
Quando os dados são copiados para uma conta de armazenamento, os dados estiverem formatados como JSON e é local em dois contêineres:

* insights-logs-operationlogs: para os registos de tráfego de pesquisa
* as métricas-insights-pt1m: para métricas

Há um blob, por hora, por contentor.

Caminho de exemplo: `resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2015/m=12/d=25/h=01/m=00/name=PT1H.json`

#### <a name="log-schema"></a>Esquema de registo
Os blobs de registos contêm os seus registos de tráfego do serviço de pesquisa.
Cada blob tem um objeto de raiz chamado **registos** que contém uma matriz de objetos de registo.
Cada blob tem registos em todas as operações que ocorreu durante a mesma hora.

| Nome | Tipo | Exemplo | Notas |
| --- | --- | --- | --- |
| hora |datetime |"2015-12-07T00:00:43.6872559Z" |TimeStamp da operação |
| resourceId |cadeia |"/ SUBSCRIÇÕES/11111111-1111-1111-1111-111111111111 /<br/>PADRÃO/RESOURCEGROUPS/FORNECEDORES /<br/> MICROSOFT. PESQUISA/SEARCHSERVICES/SEARCHSERVICE" |O ResourceId |
| operationName |cadeia |"Query.Search" |O nome da operação |
| operationVersion |cadeia |"2015-02-28" |A api-version utilizada |
| categoria |cadeia |"OperationLogs" |constante |
| resultType |cadeia |"Êxito" |Valores possíveis: Êxito ou falha |
| resultSignature |int |200 |Código de resultado HTTP |
| durationMS |int |50 |Duração da operação em milissegundos |
| propriedades |objeto |consulte a tabela seguinte |Objeto que contém dados específicos da operação |

**Esquema de propriedades**

| Nome | Tipo | Exemplo | Notas |
| --- | --- | --- | --- |
| Descrição |cadeia |"Obter /indexes('content')/docs" |Ponto final da operação |
| Consulta |cadeia |"? pesquisa = AzureSearch & $count = true e a api-version = 2015-02-28" |Os parâmetros de consulta |
| Documentos |int |42 |Número de documentos processados |
| indexName |cadeia |"testindex" |Nome do índice associado à operação |

#### <a name="metrics-schema"></a>Esquema de métricas

| Nome | Tipo | Exemplo | Notas |
| --- | --- | --- | --- |
| resourceId |cadeia |"/ SUBSCRIÇÕES/11111111-1111-1111-1111-111111111111 /<br/>PADRÃO/RESOURCEGROUPS/FORNECEDORES /<br/>MICROSOFT. PESQUISA/SEARCHSERVICES/SEARCHSERVICE" |o id de recurso |
| MetricName |cadeia |"Latência" |o nome da métrica |
| hora |datetime |"2015-12-07T00:00:43.6872559Z" |timestamp da operação |
| média |int |64 |O valor médio dos exemplos não processados no intervalo de tempo de métrica |
| mínimo |int |37 |O valor mínimo dos exemplos não processados no intervalo de tempo de métrica |
| máximo |int |78 |O valor máximo das amostras não processados no intervalo de tempo de métrica |
| total |int |258 |O valor total dos exemplos não processados no intervalo de tempo de métrica |
| count |int |4 |O número de amostras não processados, utilizado para gerar a métrica |
| intervalo de agregação |cadeia |"PT1M" |O intervalo de agregação da métrica no ISO 8601 |

Todas as métricas são comunicadas em intervalos de um minuto. Cada medição expõe valores mínimos, máximo e médios por minuto.

Para a métrica de SearchQueriesPerSecond, o valor mais baixo para consultas de pesquisa por segundo que foram registadas durante esse minuto é mínimo. O mesmo se aplica ao valor máximo. Média, é a agregação em minuto completo.
Pense neste cenário, durante um minuto: um segundo alta isto é carregar o máximo para SearchQueriesPerSecond, seguido de 58 segundos da carga média, e, finalmente, um segundo, com apenas uma consulta, que é o mínimo.

Para ThrottledSearchQueriesPercentage, mínimo, máximo, média e total, todos têm o mesmo valor: a percentagem de consultas de pesquisa que eram limitados, do número total de consultas de pesquisa durante um minuto.

## <a name="analyzing-your-data-with-power-bi"></a>Analisar os seus dados com o Power BI

Recomendamos que utilize [Power BI](https://powerbi.microsoft.com) para explorar e visualizar os seus dados. Pode ligar facilmente a sua conta de armazenamento do Azure e a começar rapidamente a analisar os seus dados.

O Azure Search fornece um [o pacote de conteúdos do Power BI](https://app.powerbi.com/getdata/services/azure-search) que permite-lhe monitorizar e compreender o seu tráfego de pesquisa com tabelas e gráficos predefinidos. Ele contém um conjunto de relatórios do Power BI que automaticamente se ligam aos seus dados e fornecem informações visuais sobre o serviço de pesquisa. Para obter mais informações, consulte a [página de ajuda do pacote de conteúdos](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/).

![Dashboard do Power BI para o Azure Search][4]

## <a name="next-steps"></a>Passos Seguintes
Revisão [dimensionar as réplicas e partições](search-limits-quotas-capacity.md) para obter orientações sobre como equilibrar a alocação de partições e réplicas para um serviço existente.

Visite [gerir o seu serviço de pesquisa no Microsoft Azure](search-manage.md) para obter mais informações sobre a administração do serviço, ou [desempenho e otimização](search-performance-optimization.md) para o guia de sintonização.

Saiba mais sobre a criação de relatórios incríveis. Ver [introdução ao Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) para obter detalhes

<!--Image references-->
[1]: ./media/search-monitor-usage/AzSearch-Monitor-BarChart.PNG
[2]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG
[3]: ./media/search-monitor-usage/AzureSearch-Enable-Monitoring.PNG
[4]: ./media/search-monitor-usage/AzureSearch-PowerBI-Dashboard.png
