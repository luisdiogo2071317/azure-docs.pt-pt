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
ms.date: 10/16/2018
ms.author: magoedte
ms.openlocfilehash: 63549768f616e60e92c853047525c18cefdaddb4
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49386283"
---
# <a name="how-to-chart-performance-with-azure-monitor-for-vms"></a>Como desempenho gráfico com o Azure Monitor para VMs
Monitor do Azure para VMs inclui um conjunto de gráficos de desempenho que visam vários indicadores chave de desempenho (KPIs) para ajudar a determinar a forma como uma máquina virtual está a efetuar. Os gráficos mostram a utilização de recursos durante um período de tempo para que possa identificar estrangulamentos de anomalias, ou mudar para um ponto de vista listagem cada máquina para ver a utilização de recursos com base na métrica selecionada. Embora haja vários elementos a serem considerados ao lidar com desempenho, Monitor do Azure para VMs se concentra no sistema operativo, conforme apresentado através do processador, memória, placas de rede e discos. Complementa a funcionalidade de monitorização de estado de funcionamento de desempenho e ajuda a expor problemas que indiquem uma falha de componente do sistema possíveis, ajuste de suporte e a otimização para alcançar a eficiência ou suportar o planeamento de capacidade.  

## <a name="multi-vm-perspective-from-azure-monitor"></a>Ponto de vista de várias VMS do Azure Monitor
Monitor do Azure, o recurso de desempenho fornece uma vista de máquina virtual com vários de todas as VMs monitorizadas implementadas em grupos de recursos nas suas subscrições ou no seu ambiente.  Para aceder a partir do Azure Monitor, execute o seguinte. 

1. No portal do Azure, selecione **Monitor**. 
2. Escolher **máquinas virtuais (pré-visualização)** no **soluções** secção.
3. Selecione o **desempenho** separador.

![Vista de lista de N principais de desempenho de informações VM](./media/monitoring-vminsights-performance/vminsights-performance-aggview-01.png)

No **Top N gráficos** separador, se tiver mais do que um Log Analytics área de trabalho, escolha a área de trabalho ativada com a solução a partir do **área de trabalho** Seletor na parte superior da página. O **grupo** Seletor irá devolver subscrições, grupos de recursos [grupos de computadores](../log-analytics/log-analytics-computer-groups.md)e conjuntos de dimensionamento VM de computadores relacionados com a área de trabalho selecionada que pode utilizar para filtrar ainda mais os resultados apresentados nos gráficos nesta página e nas outras páginas. A seleção de apenas aplica-se para o recurso de desempenho e não vão ser transferidos para o estado de funcionamento ou de mapa.  

Por predefinição, os gráficos mostram as últimas 24 horas. Utilizar o **TimeRange** Seletor, pode consultar para intervalos de tempo do histórico de até 30 dias para mostrar como o desempenho observado no passado.   

Os gráficos de utilização de capacidade de cinco mostrados na página são:

* % De utilização de CPU - mostra as máquinas de 5 principais com a maior utilização média do processador 
* Memória disponível - mostra as principais 5 máquinas com a menor quantidade média de memória disponível 
* % Utilizada de espaço em disco lógico - mostra as principais 5 máquinas com o maior espaço de disco de média utilizada % em todos os volumes de disco 
* Taxa de bytes enviados - mostra as máquinas de 5 principais com maior média de bytes enviados 
* Taxa de receção de bytes - mostra as máquinas de 5 principais com maior média de bytes enviados 

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
Métricas de desempenho ativadas como parte do Azure Monitor para VMs não incluem as regras de alerta pré-configurado. Embora haja alertas de estado de funcionamento correspondente a problemas de desempenho detetados na VM do Azure, como a alta utilização da CPU, pouca memória disponível, de baixa o espaço em disco, etc., estes alertas de estado de funcionamento só são aplicadas a todas as VMs ligadas à mesma área de trabalho do Log Analytics integrado com o Azure Monitor para as VMs. Se precisar da flexibilidade para especificar seus próprios critérios ou lógica, pode criar regras de alerta personalizadas ao seguir [criar, ver e gerir alertas ao utilizar o Azure Monitor](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). 

## <a name="next-steps"></a>Passos Seguintes
Para saber como utilizar a funcionalidade de estado de funcionamento, veja [vista do Azure Monitor de estado de funcionamento de VMs](monitoring-vminsights-health.md), ou para ver dependências de aplicações detetadas, consulte [vista de Azure Monitor para o mapa de VMs](monitoring-vminsights-maps.md). 