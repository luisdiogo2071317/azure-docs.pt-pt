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
ms.devlang: na
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: magoedte; bwren
ms.component: na
ms.openlocfilehash: 386aad94461fa3f2ceafb7564342797eefa2f086
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/21/2018
ms.locfileid: "42062102"
---
# <a name="viewing-and-analyzing-data-in-log-analytics"></a>Visualizando e analisando os dados no Log Analytics
Existem duas opções disponíveis no portal do Azure para analisar dados armazenados no Log analytics e para a criação de consultas de análise ad hoc. As consultas que crie com estes portais podem ser utilizadas para outros recursos, como alertas e dashboards.

## <a name="log-analytics-page-preview"></a>Página do log Analytics (pré-visualização)
Abra a página do Log Analytics da **registos (pré-visualização)** no menu do Log Analytics. Esta é uma nova experiência para trabalhar com dados de registo e criação de consultas. Pode obter uma introdução a este portal e inspecionar seus recursos em [começar com a página do Log Analytics no portal do Azure](query-language/get-started-analytics-portal.md).

A página do Log Analytics fornece as seguintes melhorias relativamente a [pesquisa de registos](#log-search) experiência.

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

![Portal de análises avançado](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


### <a name="firewall-requirements"></a>Requisitos de firewall
O browser requer acesso para os seguintes endereços para acessar a página do Log Analytics e o portal da análise avançada.  Se o browser estiver acessando o portal do Azure através de uma firewall, tem de ativar o acesso a estes endereços.

| Uri | IP | Portas |
|:---|:---|:---|
| portal.loganalytics.io | Dinâmica | 80,443 |
| api.loganalytics.io    | Dinâmica | 80,443 |
| docs.loganalytics.io   | Dinâmica | 80,443 |


## <a name="log-search"></a>Pesquisas de registos
Abra a página de pesquisa de registo da **Logs** no menu do Log Analytics ou a partir de **do Log Analytics** no menu do Azure Monitor. Isso é adequado para analisar dados de registo através de consultas básicas. Ele fornece várias funcionalidades para edição de consultas sem ter um conhecimento completo da linguagem de consulta.  Pode obter um resumo desses recursos no [pesquisas de registos de criar no Azure Log Analytics com a pesquisa de registos](log-analytics-log-search-log-search-portal.md). 


![Página de pesquisa de registo](media/log-analytics-log-search-portals/log-search-portal.png)


## <a name="next-steps"></a>Passos Seguintes

- Percorrer um [tutorial sobre como utilizar a pesquisa de registos](log-analytics-tutorial-viewdata.md) para saber como criar consultas usando a linguagem de consulta
- Percorrer um [lição através do portal do Advanced Analytics](query-language/get-started-analytics-portal.md) que fornece a mesma experiência que a página do Log Analytics.

