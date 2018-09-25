---
title: Monitorizar o desempenho de cluster do AKS com o Azure Monitor para contentores | Documentos da Microsoft
description: Este artigo descreve como pode ver e analisar os dados de desempenho e de registo com o Azure Monitor para contentores.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2018
ms.author: magoedte
ms.openlocfilehash: 6df7d42bc291713a815cac9f719f53136ed35b19
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956680"
---
## <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers"></a>Compreender o desempenho de cluster do AKS com o Azure Monitor para contentores
Clusters de visualizar o desempenho do seu Azure Kubernetes Service (AKS) podem ser observados de duas perspetivas com o Azure Monitor para contentores, diretamente a partir de um cluster do AKS ou ver todos os clusters do AKS numa subscrição do Azure Monitor. 

Este artigo ajuda-o a compreender a experiência de entre as duas perspetivas e como avaliar rapidamente, investigar e resolver problemas detetados.

Para obter informações sobre como ativar o Azure Monitor para contentores, consulte [carregar Monitor do Azure para contentores](monitoring-container-insights-onboard.md).

O Azure Monitor proporciona uma vista de cluster multi que mostra o estado de funcionamento de monitorizados todos os clusters do AKS implementadas em grupos de recursos nas suas subscrições e clusters do AKS não monitorizados pela solução.  Imediatamente, pode compreender o estado de funcionamento do cluster e a partir daqui pode desagregar para a página de desempenho do nó e o controlador ou pode navegar para ver os gráficos de desempenho para o cluster.  Para clusters do AKS detetados e identificado como não monitorizado, pode optar por ativar a monitorização para esse cluster em qualquer altura.  

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no [portal do Azure](https://portal.azure.com). 

## <a name="multi-cluster-view-from-azure-monitor"></a>Vista de cluster multi do Azure Monitor 
Para ver o estado de funcionamento de todos os clusters do AKS implementados, selecione **Monitor** do painel esquerdo no portal do Azure.  Sob o **Insights** secção select **contentores (pré-visualização)**.  

![Exemplo de dashboard de cluster multi de Monitor do Azure](./media/monitoring-container-insights-analyze/azmon-containers-multiview.png)

Sobre o **monitorizados clusters** separador, é possível saber o seguinte:

1. Quantos clusters estão num estado crítico ou mau estado de funcionamento, versus quantas estão em bom estado ou de relatório não (referido como um Estado desconhecido)?
2. Quantos nós, o utilizador e os pods de sistema são implementados por cluster.  

Os Estados de funcionamento definidos são: 

1. **Bom estado de funcionamento** – nenhum problema detetado para a VM e está a funcionar conforme necessário. 
2. **Crítico** – são detetados um ou mais problemas críticos, que precisam ser abordadas para restaurar o estado operacional normal conforme esperado.
3. **Aviso** -forem detetados problemas de um ou mais, que precisam ser abordadas ou a condição de estado de funcionamento pode tornar-se crítico.
4. **Desconhecido** – se o serviço não conseguiu estabelecer uma ligação com o nó ou o pod, o estado muda para um Estado desconhecido.

Estado de funcionamento calcula o estado geral do cluster como *pior de*"os três Estados com uma exceção – se qualquer um dos três Estados for *desconhecido*, irá mostrar o estado geral do cluster **desconhecido**.  

A tabela seguinte fornece uma análise detalhada do cálculo controlando os Estados de funcionamento para um cluster monitorizado na vista de cluster multi.

| |Estado |Disponibilidade |  
|-------|-------|-----------------|  
|**Pod do utilizador**| | |  
| |Bom estado de funcionamento |100% |  
| |Aviso |90 - 99% |  
| |Crítica |< 90% |  
| |Desconhecidos |Se não comunicadas nos últimos 30 minutos |  
|**Pod do sistema**| | |  
| |Bom estado de funcionamento |100% |
| |Aviso |N/A |
| |Crítica |< 100% |
| |Desconhecidos |Se não comunicadas nos últimos 30 minutos |
|**Node** | | |
| |Bom estado de funcionamento |> 85% |
| |Aviso |60 - 84% |
| |Crítica |< 60% |
| |Desconhecidos |Se não comunicadas nos últimos 30 minutos |

Na lista de clusters, pode desagregar para o **Cluster** página, ao clicar no nome do cluster, para o **nós** página de desempenho ao clicar no rollup de nós no **nós** coluna para esse cluster específico, ou desagregar para o **controladores** página de desempenho ao clicar no rollup do **pods de utilizador** ou **pods de sistema**coluna.   

## <a name="view-performance-directly-from-an-aks-cluster"></a>Ver desempenho diretamente a partir de um cluster do AKS
Acesso para o Azure Monitor para contentores está disponível diretamente a partir de um cluster do AKS, selecionando **Insights (pré-visualização)** no painel esquerdo. Ver informações sobre o seu cluster do AKS está organizado em quatro perspectivas:

- Cluster
- Nós 
- Controladores  
- Contentores

A página padrão aberto quando clica em **Insights (pré-visualização)** é **Cluster**, e inclui quatro gráficos de desempenho de linha apresenta métricas chave de desempenho do seu cluster. 

![Exemplos de gráficos de desempenho no separador de Cluster](./media/monitoring-container-insights-analyze/containers-cluster-perfview.png)

Gráfico de desempenho apresenta quatro métricas de desempenho:

- **Utilização da CPU de nó&nbsp;%**: um ponto de vista agregado da utilização da CPU para todo o cluster. Pode filtrar os resultados para o intervalo de tempo, selecionando **Avg**, **Mín**, **Max**, **percentis 50 º**, **90**, e **95** no Seletor de percentis acima do gráfico, seja individualmente ou combinado. 
- **Utilização de memória do nó&nbsp;%**: um ponto de vista agregado de utilização da memória para todo o cluster. Pode filtrar os resultados para o intervalo de tempo, selecionando **Avg**, **Mín**, **Max**, **percentis 50 º**, **90**, e **95** no Seletor de percentis acima do gráfico, seja individualmente ou combinado. 
- **Contagem de nós**: uma contagem de nós e o estado do Kubernetes. São Estados de nós do cluster representados *todos os*, *pronto*, e *não está pronto* e podem ser filtrados individualmente ou combinado no Seletor de acima do gráfico. 
- **Contagem de pod de atividade**: uma contagem de pod e o estado a partir do Kubernetes. Estados de pods representados são *todos os*, *pendente*, *em execução*, e *desconhecido* e podem ser filtrados individualmente ou combinado no Seletor de acima do gráfico. 

Quando muda para **nós**, **controladores**, e **contentores** separador apresentada automaticamente no lado direito da página é o painel de propriedades.  Mostra as propriedades do item selecionado, incluindo etiquetas definir para organizar os objetos de Kubernetes. Clique nas **>>** ligação no painel para view\hide o painel.  

![Painel de propriedades de perspetivas do exemplo Kubernetes](./media/monitoring-container-insights-analyze/perspectives-preview-pane-01.png)

À medida que expande os objetos da hierarquia, as atualizações do painel de propriedades com base no objeto selecionado. No painel também pode ver eventos de Kubernetes com pesquisas de registos predefinido ao clicar no **registos de eventos de Kubernetes do modo de exibição** link na parte superior do painel. Para obter mais informações sobre a visualização de dados de registo do Kubernetes, consulte [pesquisar registos para analisar dados](#search-logs-to-analyze-data).

Utilize o **+ Adicionar filtro** opção na parte superior da página para filtrar os resultados para a vista pelo **Service**, **nó**, ou **espaço de nomes** e o depois Selecionar o âmbito do filtro, em seguida, selecionar a partir de um dos valores mostrados na **selecionar valores** campo.  Após a configuração do filtro é aplicado globalmente ao visualizar qualquer ponto de vista do AKS cluster.  A fórmula só suporta o sinal de igual.  Pode adicionar filtros adicionais sobre aquela primeira para refinar ainda mais os resultados.  Por exemplo, se tiver especificado um filtro por **nó**, o segundo filtro deve permitir que a seleção **Service** ou **espaço de nomes**.  

![Exemplo usando o filtro para limitar os resultados](./media/monitoring-container-insights-analyze/add-filter-option-01.png)

Especificar um filtro numa guia continua a ser aplicada ao selecionar outro e é eliminado depois de clicar no **x** símbolo junto ao filtro especificado.   

Mude para o **nós** separador e a hierarquia de linha segue o modelo de objeto do Kubernetes, começando com um nó do cluster. Expanda o nó e pode ver um ou mais pods em execução no nó. Se mais de um contêiner é agrupado para um pod, eles são exibidos como a última linha na hierarquia. Também pode ver a cargas de trabalho relacionadas não pod quantas estão em execução no anfitrião se o anfitrião tiver pressão de memória ou processador.

![Hierarquia de nós do Kubernetes de exemplo na vista de desempenho](./media/monitoring-container-insights-analyze/containers-nodes-view.png)

Pode selecionar os controladores ou contentores na parte superior da página e reveja a utilização de estado e de recursos para esses objetos.  Se em vez disso, pretende rever a utilização da memória, na **métrica** na lista pendente, selecione **memória RSS** ou **conjunto de trabalho de memória**. **Memória RSS** só é suportada para Kubernetes versão 1.8 e posterior. Caso contrário, exibir os valores para **Min&nbsp; %**  como *NaN&nbsp;%*, que é um valor de tipo de dados numéricos que representa um indefinido ou valor não representável. 

![Vista de desempenho de nós de contentor](./media/monitoring-container-insights-analyze/containers-node-metric-dropdown.png)

Por predefinição, os dados de desempenho se baseia em seis últimas horas, mas pode alterar a janela utilizando o **TimeRange** opção no canto superior esquerdo. Também pode filtrar os resultados dentro do intervalo de tempo, selecionando **Avg**, **Mín**, **Max**, **percentis 50 º**, **90**, e **95** no Seletor de percentil. 

![Seleção de percentil para filtragem de dados](./media/monitoring-container-insights-analyze/containers-metric-percentile-filter.png)

No exemplo seguinte, tenha em atenção para o nó *aks-nodepool1 -*, o valor de **contentores** é 14, que é um rollup do número total de contentores implementados.

![Rollup de contentores, por exemplo de nó](./media/monitoring-container-insights-analyze/containers-nodes-containerstotal.png)

Ele pode ajudar a identificar rapidamente se tiver um equilíbrio correto de contentores entre nós do cluster. 

As informações que são apresentadas quando exibir nós são descritas na tabela a seguir:

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do anfitrião. |
| Estado | Vista de Kubernetes do Estado de nó. |
| Média&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;% | Média de percentagem de nó com base no percentil durante o período selecionado. |
| AVG, Min, Max, 50th, 90th | Valor real de média de nós com base no percentil de, durante esse período de tempo selecionado. O valor médio é medido desde o limite de CPU/memória definido para um nó; para pods e contentores é o valor de média comunicado pelo host. |
| Contentores | Número de contentores. |
| Tempo de atividade | Representa o tempo, uma vez que um nó iniciado ou foi reiniciado. |
| Controladores | Apenas para os contentores e pods. Mostra qual controlador está residindo no. Nem todos os pods estão num controlador, para alguns podem apresentar **n/d**. | 
| Média de tendência&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;% | Tendência de gráfico de barras apresenta a percentagem de métrica de percentil do controlador. |

No Seletor de, selecione **controladores**.

![Selecione controladores de exibição](./media/monitoring-container-insights-analyze/containers-controllers-tab.png)

Aqui pode ver o estado de funcionamento do desempenho dos seus controladores.

![Vista de desempenho de controladores do < nome >](./media/monitoring-container-insights-analyze/containers-controllers-view.png)

A hierarquia de linha começa com um controlador e expande o controlador. Ver um ou mais contentores. Expanda um pod e a última linha apresenta o contentor agrupado para o pod.  

As informações que são apresentadas ao ver controladores são descritas na tabela a seguir:

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do controlador.|
| Estado | O estado de rollup dos contentores se foi concluída em execução com o estado, tal como *OK*, *Terminated*, *falha* *parado*, ou *Colocada em pausa*. Se o contentor está em execução, mas o estado foi um não está corretamente apresentados ou não foi capturado pelo agente e não tenha respondido a mais de 30 minutos, o estado é *desconhecido*. São fornecidos detalhes adicionais do ícone de estado na tabela abaixo.|
| Média&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;% | Média de rollup da percentagem média de cada entidade para a métrica selecionada e percentil. |
| AVG, Min, Max, 50th, 90th  | Rollup da média da CPU millicore ou memória desempenho do contentor para o percentil selecionado. O valor médio é medido desde o limite de CPU/memória definido para um pod. |
| Contentores | Número total de contentores para o controlador ou pod. |
| Reinicia | Rollup da contagem de reinício de contentores. |
| Tempo de atividade | Representa o tempo desde o início de um contentor. |
| Nó | Apenas para os contentores e pods. Mostra qual controlador está residindo. | 
| Média de tendência&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;%| Tendência de gráfico de barras que representa a métrica de percentil do controlador. |

Os ícones no campo status indicam o estado online dos contentores:
 
| Ícone | Estado | 
|--------|-------------|
| ![Ícone de estado em execução pronto](./media/monitoring-container-insights-analyze/containers-ready-icon.png) | Em execução (pronto)|
| ![Ícone de estado aguardando ou em pausa](./media/monitoring-container-insights-analyze/containers-waiting-icon.png) | Aguardando ou em pausa|
| ![Reportou pela última vez com o ícone de estado](./media/monitoring-container-insights-analyze/containers-grey-icon.png) | Por fim comunicados em execução, mas ainda não responderam a mais de 30 minutos|
| ![Ícone de estado com êxito](./media/monitoring-container-insights-analyze/containers-green-icon.png) | Parado ou falha ao parar com êxito|

O ícone de estado apresenta uma contagem com base no que fornece o pod. Mostra os pior dois Estados, e quando paira o rato sobre o status, apresenta um Estado de rollup de todos os pods no contentor. Se não existir um estado estável, o valor de estado apresenta **(0)**. 

No Seletor de, selecione **contentores**.

![Ver os contentores selecionadas](./media/monitoring-container-insights-analyze/containers-containers-tab.png)

Aqui pode ver o estado de funcionamento do desempenho de seus contentores do Kubernetes do Azure.  

![Vista de desempenho de controladores do < nome >](./media/monitoring-container-insights-analyze/containers-containers-view.png)

As informações que são apresentadas quando exibir contêineres são descritas na tabela a seguir:

| Coluna | Descrição | 
|--------|-------------|
| Nome | O nome do controlador.|
| Estado | Estado dos contentores, se aplicável. São fornecidos detalhes adicionais do ícone de estado na tabela seguinte.|
| Média&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;% | O rollup da percentagem média de cada entidade para a métrica selecionada e percentil. |
| AVG, Min, Max, 50th, 90th  | O rollup do média da CPU millicore ou memória desempenho do contentor para o percentil selecionado. O valor médio é medido desde o limite de CPU/memória definido para um pod. |
| Pod | Contentor onde reside o pod.| 
| Nó |  Nó onde reside o contentor. | 
| Reinicia | Representa o tempo desde o início de um contentor. |
| Tempo de atividade | Representa o tempo, uma vez que um contentor foi iniciado ou reiniciado. |
| Média de tendência&nbsp;%, Min&nbsp;%, Máx.&nbsp;%, percentis 50 º&nbsp;%, 90&nbsp;% | Uma tendência de gráfico de barras que representa a percentagem média de métrica do contentor. |

Os ícones no campo status indicam os Estados online de pods, conforme descrito na tabela a seguir:
 
| Ícone | Estado |  
|--------|-------------|  
| ![Ícone de estado em execução pronto](./media/monitoring-container-insights-analyze/containers-ready-icon.png) | Em execução (pronto)|  
| ![Ícone de estado aguardando ou em pausa](./media/monitoring-container-insights-analyze/containers-waiting-icon.png) | Aguardando ou em pausa|  
| ![Reportou pela última vez com o ícone de estado](./media/monitoring-container-insights-analyze/containers-grey-icon.png) | Por fim comunicados em execução, mas ainda não responderam em mais de 30 minutos|  
| ![Ícone de estado terminada](./media/monitoring-container-insights-analyze/containers-terminated-icon.png) | Parado ou falha ao parar com êxito|  
| ![Ícone de estado com falhas](./media/monitoring-container-insights-analyze/containers-failed-icon.png) | Estado de falha |  

## <a name="container-data-collection-details"></a>Detalhes de recolha de dados de contentor
Informações de contentor recolhe diversos dados de registo e métricas de desempenho de anfitriões de contentor e contentores. Dados são recolhidos a cada três minutos.

### <a name="container-records"></a>Registos de contentor

Exemplos de registos que são recolhidos pelo Monitor do Azure para contentores e os tipos de dados que aparecem nos resultados de pesquisa de registos são apresentados na tabela a seguir:

| Tipo de dados | Tipo de dados na pesquisa de registos | Campos |
| --- | --- | --- |
| Desempenho para anfitriões e contentores | `Perf` | CounterName de computador, ObjectName, &#40;% de tempo do processador, o disco lê MB, disco escreve MB, MB de utilização de memória, rede receba Bytes, rede enviar Bytes, o processador de seg de utilização, rede&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Inventário de contentor | `ContainerInventory` | TimeGenerated, computador, nome do contentor, ContainerHostname, imagem, ImageTag, ContainerState, ExitCode, EnvironmentVar, comando, CreatedTime, StartedTime, FinishedTime, SourceSystem, ID do contentor, ImageID |
| Inventário de imagens de contentor | `ContainerImageInventory` | TimeGenerated, computador, imagem, ImageTag, ImageSize, VirtualSize, em execução, em pausa, parado, falha, SourceSystem, ImageID, TotalContainer |
| Registo de contentor | `ContainerLog` | TimeGenerated, o computador, o ID de imagem, o nome do contentor, LogEntrySource, LogEntry, SourceSystem, ID do contentor |
| Registo do serviço de contentor | `ContainerServiceLog`  | TimeGenerated, computador, TimeOfCommand, imagem, comando, SourceSystem, ID do contentor |
| Inventário de nó do contentor | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Processo de contentor | `ContainerProcess_CL` | TimeGenerated, computador, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Inventário de pods num cluster do Kubernetes | `KubePodInventory` | TimeGenerated, computador, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus, ID do contentor, ContainerName, nome, PodLabel, espaço de nomes, PodStatus, ClusterName PodIp, SourceSystem |
| Inventário de parte de nós de um cluster de Kubernetes | `KubeNodeInventory` | TimeGenerated, computador, ClusterName, ClusterId, LastTransitionTimeReady, etiquetas, estado, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Eventos de Kubernetes | `KubeEvents_CL` | TimeGenerated, computador, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, mensagem, SourceSystem | 
| Serviços no cluster do Kubernetes | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s ServiceType_s, SourceSystem | 
| Métricas de desempenho para a parte de nós de cluster do Kubernetes | Desempenho &#124; onde ObjectName = = "K8SNode" | CounterName de computador, ObjectName, &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Métricas de desempenho para a parte de contentores de cluster do Kubernetes | Desempenho &#124; onde ObjectName = = "K8SContainer" | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 

## <a name="search-logs-to-analyze-data"></a>Registos de pesquisa para analisar dados
O log Analytics pode ajudá-lo a analisar as tendências, diagnosticar afunilamentos, previsão, ou correlacionar dados que podem ajudar a determinam se a configuração de cluster atual é satisfatória. Pesquisas de registos predefinidas são fornecidas por si para começar imediatamente a utilizar ou personalizar para retornar as informações da maneira como desejar. 

Pode efetuar análises interativas de dados na área de trabalho ao selecionar o **registos de eventos de Kubernetes do modo de exibição** ou **ver registos de contentor** opção no painel de pré-visualização. O **pesquisa de registos** é apresentada a página à direita da página do portal do Azure que estavam no.

![Analisar dados no Log Analytics](./media/monitoring-container-insights-analyze/container-health-log-search-example.png)   

A saída de registos de contentor é reencaminhada para o Log Analytics são STDOUT e STDERR. Porque o Azure Monitor está a monitorizar o Kubernetes gerido pelo Azure (AKS), o sistema do Kube não é coletado hoje devido ao grande volume de dados gerados. 

### <a name="example-log-search-queries"></a>Consultas de pesquisa de registo de exemplo
Muitas vezes é útil criar consultas que começam com um ou dois exemplos em, em seguida, modificá-las para satisfazer as suas necessidades. Para ajudar a criar consultas mais avançadas, pode experimentar com as seguintes consultas de exemplo:

| Consulta | Descrição | 
|-------|-------------|
| ContainerInventory<br> &#124;Computador, nome, imagem, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime do projeto<br> &#124;tabela de composição | Listar todas as informações de ciclo de vida de um contentor| 
| KubeEvents_CL<br> &#124;onde not(isempty(Namespace_s))<br> &#124;Ordenar por TimeGenerated desc<br> &#124;tabela de composição | Eventos de Kubernetes|
| ContainerImageInventory<br> &#124;resumir AggregatedValue = count () by imagem, ImageTag, está em execução | Inventário de imagens | 
| **No Advanced Analytics, selecione os gráficos de linhas**:<br> Desempenho<br> &#124;onde ObjectName = = "Contentor" e CounterName = = "% tempo do processador"<br> &#124;resumir AvgCPUPercent = avg(CounterValue) por bin (TimeGenerated, 30m), InstanceName | Contentor da CPU | 
| **No Advanced Analytics, selecione os gráficos de linhas**:<br> Desempenho &#124; onde ObjectName = = "Contentor" e CounterName = = "MB de utilização de memória"<br> &#124;resumir AvgUsedMemory = avg(CounterValue) por bin (TimeGenerated, 30m), InstanceName | Memória de contentor |
