---
title: Ver e analisar os dados no Azure Log Analytics | Documentos da Microsoft
description: Este artigo descreve os portais que pode utilizar pesquisas de registos do Azure Log Analytics para criar e editar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 1d6a3ef4094bed92e8c40f2416b637f0830c3845
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52959296"
---
# <a name="viewing-and-analyzing-data-in-log-analytics"></a>Visualizando e analisando os dados no Log Analytics
Existem duas opções disponíveis no portal do Azure para analisar dados armazenados no Log analytics e para a criação de consultas de análise ad hoc. As consultas que crie com estes portais podem ser utilizadas para outros recursos, como alertas e dashboards.

## <a name="log-analytics-page"></a>Página do log Analytics
Abra a página do Log Analytics da **registos** no menu do Log Analytics. Esta é uma nova experiência para trabalhar com dados de registo e criação de consultas. Pode obter uma introdução a este portal e inspecionar seus recursos em [começar com a página do Log Analytics no portal do Azure](../../azure-monitor/log-query/get-started-portal.md).

A página do Log Analytics fornece as seguintes melhorias relativamente a [pesquisa de registos (clássico)](#log-search-classic) experiência.

* Vários separadores – criar guias separados para trabalhar com várias consultas.
* Visualizações ricas – diversas opções de criação de gráficos.
* Melhor Intellisense e a linguagem de preenchimento automático.
* Realce de sintaxe – melhora a legibilidade das consultas. 
* Explorador de consultas – acesso guardado, consulta e funções.
* Esquema ver – rever a estrutura dos seus dados para ajudar a escrever consultas.
* Partilhar – criar ligações para consultas ou consultas de pin para qualquer dashboard partilhado do Azure.
* Análise do smart - identifica os picos de seus gráficos e uma análise rápida da causa.
* Seleção de coluna – Ordenar e agrupar colunas nos resultados da consulta.

> [!NOTE]
> A página do Log Analytics tem a mesma funcionalidade que o portal da análise avançada que é uma ferramenta externa fora do portal do Azure. O portal da análise avançada ainda está disponível, mas ligações e outras referências a ele no portal do Azure estão a ser substituídas com esta nova página.

![Portal de análises avançado](media/portals/advanced-analytics-portal.png)

### <a name="resource-logs"></a>Registos de recursos
A nova experiência de Log Analytics integra-se a vários recursos do Azure como máquinas virtuais. Isso significa que pode abrir a página do Log Analytics diretamente através do menu de monitorização do recurso sem mudar para o Azure Monitor ou o Log Analytics e perder o contexto de recursos. **Registos** ainda não tiver sido ativada para todos os recursos do Azure, mas vão começar a aparecer no menu do portal para os recursos de diferentes tipos.

Quando abrir o Log Analytics a partir de um recurso específico, ele tem um âmbito automaticamente para criar registos apenas desse recurso.   Se quiser escrever uma consulta que inclui outros registos, em seguida, precisaria para abri-lo no menu do Log Analytics ou no Azure Monitor.

As seguintes opções ainda não estão disponíveis por meio do modo de exibição do Log Analytics:

- Guardar
- Definir alerta
- Explorador de consultas
- Mudar para a área de trabalho/recursos diferentes (atualmente não planejado)


### <a name="firewall-requirements"></a>Requisitos de firewall
O browser requer acesso para os seguintes endereços para acessar a página do Log Analytics e o portal da análise avançada.  Se o browser estiver acessando o portal do Azure através de uma firewall, tem de ativar o acesso a estes endereços.

| URI | IP | Portas |
|:---|:---|:---|
| portal.loganalytics.io | Dinâmica | 80,443 |
| api.loganalytics.io    | Dinâmica | 80,443 |
| docs.loganalytics.io   | Dinâmica | 80,443 |


## <a name="log-search-classic"></a>Pesquisa de registos (clássico)
Abra a página de pesquisa de registo da **registos (clássico)** no menu do Log Analytics ou a partir de **do Log Analytics** no menu do Azure Monitor. Esta é a página clássica usada para trabalhar com consultas do Log Analytics que não tenha o adicionais, os recursos da [página do Log Analytics](#log-analytics-page) listados acima.



![Página de pesquisa de registo](media/portals/log-search-portal.png)


## <a name="next-steps"></a>Passos Seguintes

- Percorrer um [tutorial sobre como utilizar a pesquisa de registos](../../azure-monitor/learn/tutorial-viewdata.md) para saber como criar consultas usando a linguagem de consulta
- Percorrer um [lição através do portal do Advanced Analytics](../../azure-monitor/log-query/get-started-portal.md) que fornece a mesma experiência que a página do Log Analytics.

