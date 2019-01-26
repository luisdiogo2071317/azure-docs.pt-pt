---
title: Monitorizar as estatísticas de utilização e consulta recursos para um serviço de pesquisa - Azure Search
description: Obter métricas de atividade de consulta, o consumo de recursos e outros dados de sistema de um serviço Azure Search.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: af2a9cd7f834f5c6f70a78d94e8826de2584127d
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/26/2019
ms.locfileid: "55076384"
---
# <a name="monitor-an-azure-search-service-in-azure-portal"></a>Monitorizar um serviço de Azure Search no portal do Azure

Na página de descrição geral do seu serviço Azure Search, pode ver dados de sistema sobre a utilização de recursos, além de métricas de consulta, como consultas por segundo (QPS), a latência da consulta e a percentagem de pedidos que foram limitados. Além disso, pode utilizar o portal para tirar partido de uma gama de capacidades na plataforma do Azure para a recolha de dados mais aprofundada de monitorização. 

Este artigo identifica e compara as opções disponíveis para o registo de operações de pesquisa do Azure. Ele inclui instruções para habilitar o Registro em log e de registo de armazenamento e como aceder a informações atividade de utilizador e serviço.

A definição dos registos é útil para self-diagnostics e preservar um histórico das operações de serviço. Internamente, os registos existe durante um curto período de tempo, suficiente para investigação e análise, se enviar um pedido de suporte. Se pretender controlar o armazenamento de informações de registo para o seu serviço, deve configurar uma das soluções descritas neste artigo.

## <a name="metrics-at-a-glance"></a>Indicadores de relance

**Utilização** e **monitorização** seções incorporadas no Descrição geral visualizar o consumo de armazenamento e consultar métricas de execução. Estas informações ficam disponíveis assim que começar a utilizar o serviço, sem qualquer configuração necessária. Esta página é atualizada intervalos de poucos minutos. Se finalizar decisões sobre [qual dos escalões a utilizar para cargas de trabalho de produção](search-sku-tier.md), ou se pretende [ajustar o número de partições e réplicas do Active Directory](search-capacity-planning.md), estas métricas podem ajudá-lo com essas decisões por mostrando a como rapidamente os recursos são consumidos e a eficiência com que a configuração atual lida com a carga existente.

O **utilização** Guia mostra-lhe a disponibilidade de recursos em relação ao atual [limites](search-limits-quotas-capacity.md). A ilustração seguinte é para o serviço gratuito, o que está limitado a 3 objetos de cada tipo e a 50 MB de armazenamento. Um serviço básico ou Standard tem limites mais elevados e, se aumentar as contagens de partição, máximo de armazenamento aumenta proporcionalmente.

![Estado de utilização em relação ao limites em vigor](./media/search-monitor-usage/usage-tab.png
 "estado de utilização em relação ao limites em vigor")

## <a name="queries-per-second-qps-and-other-metrics"></a>Consultas por segundo (QPS) e outras métricas

O **monitorização** separador mostra médias móveis para métricas, como pesquisa *consultas por segundo* (QPS), agregados por minuto. 
*Latência de pesquisa* é a quantidade de tempo o serviço de pesquisa necessária para processar consultas de pesquisa, agregadas por cada minuto. *Percentagem de consulta de pesquisa limitada* (não mostrado) é a porcentagem de consultas de pesquisa que eram limitados, também agregados por cada minuto.

Esses números são aproximados e foram concebidos para lhe dar uma ideia geral de como seu sistema está a servir pedidos. QPS real pode ser superior ou inferior ao número comunicado no portal.

![Consultas por segundo atividade](./media/search-monitor-usage/monitoring-tab.png "consultas por segundo atividade")

## <a name="activity-logs"></a>Registos de atividade

O **registo de atividades** recolhe informações do Azure Resource Manager. Exemplos de informações encontradas no registo de atividades incluem criar ou eliminar um serviço, a atualizar um grupo de recursos, a verificação de disponibilidade do nome ou obter uma chave de acesso de serviço de processamento do pedido. 

Pode aceder a **registo de atividades** do painel de navegação à esquerda ou de notificações no comando janela superior de barra ou a partir da **diagnosticar e resolver problemas** página.

Para tarefas de versão, como criar um índice ou a eliminação de uma origem de dados, verá notificações genéricas, como "Obter chave de administrador" para cada solicitação, mas não a ação específica em si. Para este nível de informações, tem de ativar uma solução de monitorização do suplemento.

## <a name="add-on-monitoring-solutions"></a>Soluções de monitorização do suplemento

O Azure Search não armazena quaisquer dados para além dos objetos que gere, o que significa que o registo de dados devem ser armazenados externamente. Pode configurar qualquer um dos recursos abaixo se pretender manter os dados de registo. 

A tabela seguinte compara as opções para armazenar os registos e a adição de monitorização profunda das operações de serviço e cargas de trabalho de consulta através do Application Insights.

| Recurso | Utilizado para |
|----------|----------|
| [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) | Eventos registrados e métricas de consulta, com base nos esquemas abaixo, correlacionadas com eventos de utilizador na sua aplicação. Esta é a única solução que considera ações do usuário ou sinais, eventos de mapeamento a partir da Pesquisa iniciada pelo usuário, em oposição ao filtrar pedidos submetidos por código do aplicativo. Para utilizar esta abordagem, copiar-colar o código de instrumentação nos seus arquivos de origem a informações de pedido de rota para o Application Insights. Para obter mais informações, consulte [a análise de tráfego de pesquisa](search-traffic-analytics.md). |
| [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) | Eventos registrados e métricas de consulta, com base nos esquemas abaixo. Os eventos são registados numa área de trabalho do Log Analytics. Pode executar consultas em relação a uma área de trabalho para devolver informações detalhadas do log. Para obter mais informações, consulte [introdução ao Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata) |
| [Armazenamento de blobs](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) | Eventos registrados e métricas de consulta, com base nos esquemas abaixo. Os eventos são registados para um contentor de BLOBs e armazenados em ficheiros JSON. Utilize um editor de JSON para ver o conteúdo do ficheiro.|
| [Hub de Eventos](https://docs.microsoft.com/azure/event-hubs/) | Eventos registrados e métricas de consulta, com base em esquemas documentados neste artigo. Escolha esta opção como um serviço de recolha de dados alternativo para os registos de muito grandes. |



O Log Analytics e o armazenamento de BLOBs estão disponíveis como serviço partilhado gratuito para que pode experimentar, sem encargos durante o tempo de vida da sua subscrição do Azure. A secção seguinte explica os passos para ativar e utilizar o armazenamento de Blobs do Azure para recolher e aceder aos dados de log criados por operações de pesquisa do Azure.

## <a name="enable-logging"></a>Ativar registo

Para cargas de trabalho de indexação e consulta o registo está desativada por predefinição e depende de soluções de suplemento para a infra-estrutura de log e de armazenamento externo de longo prazo. Por si só, os únicos dados persistentes no Azure Search são índices, para que os registos devem ser armazenados em outro lugar.

Nesta secção, irá aprender como utilizar o armazenamento de BLOBs para armazenar dados de métricas e eventos registados.

1. [Criar uma conta de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) se ainda não tiver uma. Pode colocá-lo no mesmo grupo de recursos do Azure Search para simplificar de limpeza do wsu mais tarde se pretender eliminar todos os recursos utilizados neste exercício.

2. Abra sua página de descrição geral do serviço de pesquisa. No painel de navegação à esquerda, desloque para baixo até **monitorização** e clique em **Ativar monitorização**.

   ![Ativar a monitorização](./media/search-monitor-usage/enable-monitoring.png "ativar a monitorização")

3. Escolha os dados que pretende exportar: Os registos, métricas ou ambos. Pode copiá-lo para uma conta de armazenamento, enviá-lo para um hub de eventos ou exportá-lo para o Log Analytics.

   Para o arquivo para o armazenamento de BLOBs, deve existir apenas a conta de armazenamento. Contentores e blobs serão criadas quando os dados de registo são exportados.

   ![Arquivo de armazenamento de BLOBs de configurar](./media/search-monitor-usage/configure-blob-storage-archive.png "arquivo de armazenamento de BLOBs de configurar")

4. Guarde o perfil.

5. Teste o registo ao criar ou a exclusão de objetos (cria eventos de registo) e, ao submeter consultas (gera métricas). 

O registo está ativado depois de guardar o perfil, contentores, apenas são criados quando existe um evento para o registo ou medida. Pode demorar alguns minutos para que os contentores a aparecer. Quando os dados são copiados para uma conta de armazenamento, os dados são formatados como JSON e colocados em dois contêineres:

* insights-logs-operationlogs: para os registos de tráfego de pesquisa
* as métricas-insights-pt1m: para métricas

Pode usar [Visual Studio Code](#Download-and-open-in-Visual-Studio-Code) ou outro editor de JSON para ver os ficheiros. Há um blob, por hora, por contentor.

### <a name="example-path"></a>Caminho de exemplo

```
resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2018/m=12/d=25/h=01/m=00/name=PT1H.json
```

## <a name="log-schema"></a>Esquema de registo
Os BLOBs que contém os seus registos de tráfego do serviço de pesquisa são estruturados, conforme descrito nesta secção. Cada blob tem um objeto de raiz chamado **registos** que contenha uma matriz de objetos de registo. Cada blob contém registos para todas as operações que ocorreu durante a mesma hora.

| Nome | Tipo | Exemplo | Notas |
| --- | --- | --- | --- |
| hora |datetime |"2018-12-07T00:00:43.6872559Z" |TimeStamp da operação |
| resourceId |cadeia |"/ SUBSCRIÇÕES/11111111-1111-1111-1111-111111111111 /<br/>PADRÃO/RESOURCEGROUPS/FORNECEDORES /<br/> MICROSOFT. PESQUISA/SEARCHSERVICES/SEARCHSERVICE" |O ResourceId |
| operationName |cadeia |"Query.Search" |O nome da operação |
| operationVersion |cadeia |"2017-11-11" |A api-version utilizada |
| categoria |cadeia |"OperationLogs" |constante |
| resultType |cadeia |"Êxito" |Valores possíveis: Êxito ou falha |
| resultSignature |int |200 |Código de resultado HTTP |
| durationMS |int |50 |Duração da operação em milissegundos |
| propriedades |objeto |consulte a tabela seguinte |Objeto que contém dados específicos da operação |

**Esquema de propriedades**

| Nome | Tipo | Exemplo | Notas |
| --- | --- | --- | --- |
| Descrição |cadeia |"Obter /indexes('content')/docs" |Ponto final da operação |
| Consulta |cadeia |"?search=AzureSearch&$count=true&api-version=2017-11-11" |Os parâmetros de consulta |
| Documentos |int |42 |Número de documentos processados |
| indexName |cadeia |"testindex" |Nome do índice associado à operação |

## <a name="metrics-schema"></a>Esquema de métricas

As métricas são capturadas para pedidos de consulta.

| Nome | Tipo | Exemplo | Notas |
| --- | --- | --- | --- |
| resourceId |cadeia |"/ SUBSCRIÇÕES/11111111-1111-1111-1111-111111111111 /<br/>PADRÃO/RESOURCEGROUPS/FORNECEDORES /<br/>MICROSOFT. PESQUISA/SEARCHSERVICES/SEARCHSERVICE" |o id de recurso |
| MetricName |cadeia |"Latência" |o nome da métrica |
| hora |datetime |"2018-12-07T00:00:43.6872559Z" |timestamp da operação |
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

## <a name="download-and-open-in-visual-studio-code"></a>Transfira e abra no Visual Studio Code

Pode utilizar qualquer editor de JSON para ver o ficheiro de registo. Se não tiver uma, é recomendável [Visual Studio Code](https://code.visualstudio.com/download).

1. No portal do Azure, abra a sua conta de armazenamento. 

2. No painel de navegação à esquerda, clique em **Blobs**. Verá **insights-logs-operationlogs** e **insights-métricas-pt1m**. Estes contentores são criados pelo Azure Search, quando os dados de registo são exportados para o armazenamento de Blobs.

3. Clique para baixo a hierarquia de pastas até alcançar o ficheiro. JSON.  Utilize o menu de contexto para transferir o ficheiro.

Depois do ficheiro é transferido, abra-o num editor de JSON para exibir o conteúdo.

## <a name="get-sys-info-apis"></a>Obter APIs de sys-info
A API de REST do Azure Search e o SDK do .NET fornecem acesso programático às informações de métricas, índice e indexador de serviço e contagem de documentos.

* [Obter estatísticas de serviços](/rest/api/searchservice/get-service-statistics)
* [Obter estatísticas de índice](/rest/api/searchservice/get-index-statistics)
* [Contagem de documentos](/rest/api/searchservice/count-documents)
* [Obter estado do indexador](/rest/api/searchservice/get-indexer-status)

Para ativar a utilizar o PowerShell ou a CLI do Azure, consulte a documentação [aqui](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

## <a name="next-steps"></a>Passos Seguintes

[Gerir o seu serviço de pesquisa no Microsoft Azure](search-manage.md) para obter mais informações sobre a administração de serviços e [desempenho e otimização](search-performance-optimization.md) para o guia de sintonização.