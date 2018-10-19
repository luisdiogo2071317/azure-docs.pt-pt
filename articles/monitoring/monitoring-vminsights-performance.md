---
title: Como gráfico de desempenho com o Azure Monitor para VMs | Documentos da Microsoft
description: O desempenho é uma funcionalidade do Monitor para VMs do Azure que Deteta componentes da aplicação em sistemas Windows e Linux e mapeia a comunicação entre serviços automaticamente. Este artigo fornece detalhes sobre como usá-lo numa variedade de cenários.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: magoedte
ms.openlocfilehash: a63a9d22638231aa076cc4ced9030a378d0c36e4
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429495"
---
# <a name="how-to-chart-performance-with-azure-monitor-for-vms"></a>Como desempenho gráfico com o Azure Monitor para VMs
Monitor do Azure para VMs inclui um conjunto de gráficos de desempenho que visam vários indicadores chave de desempenho (KPIs) para ajudar a determinar a forma como uma máquina virtual está a efetuar. Os gráficos mostram a utilização de recursos durante um período de tempo para que possa identificar estrangulamentos de anomalias, ou mudar para um ponto de vista listagem cada máquina para ver a utilização de recursos com base na métrica selecionada. Embora haja vários elementos a serem considerados ao lidar com desempenho, Monitor do Azure para VMs se concentra no sistema operativo, conforme apresentado através do processador, memória, placas de rede e discos. Complementa a funcionalidade de monitorização de estado de funcionamento de desempenho e ajuda a expor problemas que indiquem uma falha de componente do sistema possíveis, ajuste de suporte e a otimização para alcançar a eficiência ou suportar o planeamento de capacidade.  

## <a name="multi-vm-perspective-from-azure-monitor"></a>Ponto de vista de várias VMS do Azure Monitor
Monitor do Azure, o recurso de desempenho fornece uma vista de máquina virtual com vários de todas as VMs monitorizadas implementadas em grupos de recursos nas suas subscrições ou no seu ambiente.  Para aceder a partir do Azure Monitor, execute os seguintes passos. 

1. No portal do Azure, selecione **Monitor**. 
2. Escolher **máquinas virtuais (pré-visualização)** no **soluções** secção.
3. Selecione o **desempenho** separador.

![Vista de lista de N principais de desempenho de informações VM](./media/monitoring-vminsights-performance/vminsights-performance-aggview-01.png)

No **Top N gráficos** separador, se tiver mais do que um Log Analytics área de trabalho, escolha a área de trabalho ativada com a solução a partir do **área de trabalho** Seletor na parte superior da página. O **grupo** Seletor irá devolver subscrições, grupos de recursos [grupos de computadores](../log-analytics/log-analytics-computer-groups.md)e conjuntos de dimensionamento de máquinas virtuais de computadores relacionados com a área de trabalho selecionada que pode utilizar para filtrar ainda mais resultados apresentados nos gráficos nesta página e nas outras páginas. A seleção de apenas aplica-se para o recurso de desempenho e não vão ser transferidos para o estado de funcionamento ou de mapa.  

Por predefinição, os gráficos mostram as últimas 24 horas. Utilizar o **TimeRange** Seletor, pode consultar para intervalos de tempo do histórico de até 30 dias para mostrar como o desempenho observado no passado.   

Os gráficos de utilização de capacidade de cinco mostrados na página são:

* % De utilização de CPU - mostra as principais cinco máquinas com a maior utilização média do processador 
* Memória disponível - mostra as principais cinco máquinas com a menor quantidade média de memória disponível 
* % Utilizada de espaço em disco lógico - mostra as principais cinco máquinas com o maior espaço de disco de média utilizada % em todos os volumes de disco 
* Taxa de bytes enviados - mostra as máquinas de cinco principais com maior média de bytes enviados 
* Taxa de receção de bytes - mostra as máquinas de cinco principais com maior média de bytes enviados 

Clicar no canto superior direito de qualquer um dos cinco gráficos abrirá **lista de N principais** vista.  Aqui pode ver a utilização de recursos para essa métrica de desempenho por VM individual numa vista de lista e que o machine tendência da mais alta.  

![Vista de lista de N principal para uma métrica de desempenho selecionados](./media/monitoring-vminsights-performance/vminsights-performance-topnlist-01.png)

Quando clica na máquina virtual, o **propriedades** painel é expandido no lado direito para mostrar as propriedades do item selecionado, tais como informações de sistema comunicadas pelo sistema operativo, as propriedades da VM do Azure, etc. Clicar em uma das opções sob o **ligações rápidas** secção irá redirecioná-lo para essa funcionalidade diretamente a partir da VM selecionada.  

![Painel de propriedades da máquina virtual](./media/monitoring-vminsights-performance/vminsights-properties-pane-01.png)

Mude para o **gráficos agregados** separador para ver as métricas de desempenho filtrado por medidas de percentis ou uma média.  

![Vista de desempenho agregado de informações da VM](./media/monitoring-vminsights-performance/vminsights-performance-aggview-02.png)

São fornecidos os seguintes gráficos de utilização de capacidade:

* % De utilização de CPU - está predefinida que mostra o percentil 95 de médio e superior 
* Memória disponível - padrões que mostra o percentil médio, principal 5 e 10 
* % Utilizada de espaço em disco lógico - está predefinida que mostra o médio e 95 º percentil 
* Bytes enviados taxa - padrões que mostra a média de bytes enviada 
* Taxa de receção de bytes - padrões que mostra a média de bytes recebida

Também pode alterar a granularidade dos gráficos dentro do intervalo de tempo, selecionando **Avg**, **Mín**, **Max**, **percentis 50 º**,  **90**, e **95** no Seletor de percentil.   

Para ver a utilização de recursos individual VM numa vista de lista e ver a tendência da qual máquina com maior utilização, selecione o **lista de N principais** separador.  O **lista de N principais** página mostra as principais 20 máquinas ordenadas por mais utilizados por percentil 95 para a métrica *% de utilização da CPU*.  Pode ver mais máquinas, selecionando **carga mais**, e os resultados se expandem para mostrar as máquinas da parte superior a 500. 

>[!NOTE]
>A lista não é possível mostrar a mais de 500 máquinas cada vez.  
>

![Exemplo de página de lista de N principal](./media/monitoring-vminsights-performance/vminsights-performance-topnlist-01.png)

Para filtrar os resultados numa máquina virtual específica na lista, introduza o respetivo nome de computador no **procurar por nome** caixa de texto.  

Se seria em vez disso, ver a utilização de uma métrica de desempenho diferentes, do **métrica** selecione na lista pendente **memória disponível**, **% de espaço em disco lógico utilizado**,  **Recebidos bytes/s de rede**, ou **bytes/s de rede enviados** e a lista de atualizações para mostrar um âmbito para esta métrica de utilização.  

Selecionar uma máquina virtual a partir da lista abre o **propriedades** painel no lado direito da página e aqui, pode selecionar **detalhes de desempenho**.  O **detalhe da Máquina Virtual** é aberta a página e tem um âmbito para essa VM, semelhante na experiência ao aceder ao desempenho da VM informações diretamente da VM do Azure.  

## <a name="view-performance-directly-from-an-azure-vm"></a>Ver desempenho diretamente a partir de uma VM do Azure
Para aceder diretamente a partir de uma máquina virtual, faça o seguinte.

1. No portal do Azure, selecione **máquinas virtuais**. 
2. Na lista, escolha uma VM e, no **monitorização** , escolha **Insights (pré-visualização)**.  
3. Selecione o **desempenho** separador. 

Esta página não apenas inclui gráficos de utilização de desempenho, mas também uma tabela que mostra para cada disco lógico detetados, sua capacidade, utilização e total média por cada medida.  

São fornecidos os seguintes gráficos de utilização de capacidade:

* % De utilização de CPU - está predefinida que mostra o percentil 95 de médio e superior 
* Memória disponível - padrões que mostra o percentil médio, principal 5 e 10 
* % Utilizada de espaço em disco lógico - está predefinida que mostra o médio e 95 º percentil 
* IOPS - padrões que mostra o percentil 95 e médio de disco lógico
* Lógica disco MB/s - padrões que mostra o médio e 95 º percentil
* % Max lógico disco utilizado - padrões que mostra o médio e 95 º percentil
* Bytes enviados taxa - padrões que mostra a média de bytes enviada 
* Taxa de receção de bytes - padrões que mostra a média de bytes recebida

![VM insights desempenho diretamente a partir da VM ver](./media/monitoring-vminsights-performance/vminsights-performance-directvm-01.png)

## <a name="alerting-and-alert-management"></a>Gestão de alerta e alerta 
Métricas de desempenho ativadas como parte do Azure Monitor para VMs não incluem as regras de alerta pré-configurado. Embora haja [alertas de estado de funcionamento](monitoring-vminsights-health.md#alerting-and-alert-management) correspondentes a problemas de desempenho detetados na sua VM do Azure, como alta utilização da CPU, pouca memória disponível, disco e/s, espaço insuficiente no disco, etc., estes alertas de estado de funcionamento só são aplicadas a todas as VMs ligado à área de trabalho do Log Analytics mesmo ativada para o Azure Monitor para as VMs. 

No entanto, podemos pode apenas recolher e armazenar um subconjunto das métricas de desempenho que necessita na área de trabalho do Log Analytics. Se a sua estratégia de monitorização requer a análise ou alertas que incluem outras métricas de desempenho para avaliar efetivamente capacidade ou estado de funcionamento da máquina virtual, ou tem a flexibilidade para especificar seus próprios critérios ou lógica de alerta, pode configurar [coleção dos contadores de desempenho](../log-analytics/log-analytics-data-sources-performance-counters.md?toc=/azure/azure-monitor/toc.json) no Log Analytics e definir [alertas de registo](../monitoring-and-diagnostics/alert-log.md?toc=/azure/azure-monitor/toc.json). Embora o Log Analytics permite-lhe efetuar análises complexas com outros tipos de dados e fornecer uma retenção mais longa para suportar a análise de tendências, métricas por outro lado, são leves e com capacidade de oferecer suporte a cenários em tempo real em tempo quase. Eles são coletados pela [agente do diagnóstico do Azure](../virtual-machines/windows/monitor.md) e armazenado no arquivo de métricas do Azure Monitor, permitindo-lhe criar alertas com menor latência e a um custo menor.

Reveja a descrição geral dos [coleção de métricas e registos com o Azure Monitor](monitoring-data-collection.md?toc=/azure/azure-monitor/toc.json) para compreender melhor as diferenças fundamentais e outras considerações antes de configurar a coleção destas métricas adicionais e regras de alerta.  

## <a name="next-steps"></a>Passos Seguintes
Para saber como utilizar a funcionalidade de estado de funcionamento, veja [vista do Azure Monitor de estado de funcionamento de VMs](monitoring-vminsights-health.md), ou para ver dependências de aplicações detetadas, consulte [vista de Azure Monitor para o mapa de VMs](monitoring-vminsights-maps.md). 