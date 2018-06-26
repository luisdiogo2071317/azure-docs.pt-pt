---
title: Pesquisa em recursos com o Log Analytics do Azure | Microsoft Docs
description: Este artigo descreve como pode consultar relativamente aos recursos a partir de várias áreas de trabalho e aplicações do App Insights na sua subscrição.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: magoedte
ms.openlocfilehash: 52c3914cc1b51bf7c2a6d0fbf28dc0bf7756e749
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751450"
---
# <a name="perform-cross-resource-log-searches-in-log-analytics"></a>Efetuar pesquisas de registo de recursos na análise de registos  

Anteriormente com o Log Analytics do Azure, foi apenas analisar dados a partir da área de trabalho atual e limitada a capacidade de consulta em várias áreas de trabalho definidas na sua subscrição.  Além disso, pode apenas pesquisar itens de telemetria recolhidas a partir da sua aplicação baseada na web com o Application Insights diretamente no Application Insights ou a partir do Visual Studio.  Isto também efetuada-um desafio nativamente analisar operacional e dados da aplicação em conjunto.   

Agora, pode consultar não só em várias áreas de trabalho de análise de registos, mas também dados a partir de uma aplicação específica do Application Insights no mesmo grupo de recursos, outro grupo de recursos ou outra subscrição. Este procedimento fornece uma vista de todo sistema dos seus dados.  Estes tipos de consultas só pode executar o [portal avançada](log-analytics-log-search-portals.md#advanced-analytics-portal), não no portal do Azure. O número de recursos (áreas de trabalho de análise de registos e aplicação Application Insights) que pode incluir numa única consulta está limitado a 100. 

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Consultar em áreas de trabalho de análise de registos e do Application Insights
Para fazer referência a outra área de trabalho na sua consulta, utilize o [ *área de trabalho* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) identificador e para uma aplicação do Application Insights, utilize o [ *aplicação* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())identificador.  

### <a name="identifying-workspace-resources"></a>Identificar recursos da área de trabalho
Os exemplos seguintes demonstram as consultas em áreas de trabalho de análise de registos para devolvem resumidas contagens de atualizações da tabela da atualização na área de trabalho com o nome *contosoretail-it*. 

Identificar uma área de trabalho pode ser conseguido de várias formas:

* Nome de recurso - é um nome legível por humanos da área de trabalho, por vezes referido como *nome do componente*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Identificar uma área de trabalho por nome de parte do princípio de exclusividade em todas as subscrições acessíveis. Se tiver várias aplicações com o nome especificado, a consulta falha devido a ambiguidade. Neste caso, tem de utilizar um do outros identificadores.

* Nome qualificado - é o "nome completo" da área de trabalho, composto pelo nome da subscrição, o grupo de recursos e o nome de componente este formato: *resourceGroup/subscriptionName/componentName*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Porque os nomes de subscrição do Azure não são exclusivos, este identificador pode ser ambígua. 
    >

* ID da área de trabalho - um ID de área de trabalho é o identificador exclusivo, imutável, atribuído a cada área de trabalho representada como um identificador exclusivo global (GUID).

    `workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update | count`

* Recurso ID de Azure – identidade exclusiva definido pelo Azure da área de trabalho. Utilize o ID de recurso quando o nome de recurso é ambíguo.  Para áreas de trabalho, é o formato: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Áreas de trabalho/OperationalInsights/componentName*.  

    Por exemplo:
    ``` 
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

### <a name="identifying-an-application"></a>Identificar uma aplicação
Os exemplos seguintes devolvem uma contagem de pedidos efetuados em relação a uma aplicação com o nome resumida *fabrikamapp* no Application Insights. 

Identificar uma aplicação no Application Insights pode ser conseguido com a *app(Identifier)* expressão.  O *identificador* argumento especifica a aplicação utilizando um dos seguintes:

* Nome de recurso - é um nome legível humano da aplicação, por vezes referido como o *nome do componente*.  

    `app("fabrikamapp")`

* Nome qualificado - é o "nome completo" da aplicação e composto pelo nome da subscrição, o grupo de recursos e o nome de componente este formato: *resourceGroup/subscriptionName/componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Porque os nomes de subscrição do Azure não são exclusivos, este identificador pode ser ambígua. 
    >

* ID - o GUID da aplicação da aplicação.

    `app("b459b4f6-912x-46d5-9cb1-b43069212ab4").requests | count`

* Recurso ID de Azure - identidade exclusiva definido pelo Azure da aplicação. Utilize o ID de recurso quando o nome de recurso é ambíguo. O formato é: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Componentes/OperationalInsights/componentName*.  

    Por exemplo:
    ```
    app("/subscriptions/b459b4f6-912x-46d5-9cb1-b43069212ab4/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

## <a name="next-steps"></a>Passos Seguintes

Reveja o [referência de pesquisa de registo de análise de registos](https://docs.loganalytics.io/docs/Language-Reference) para ver todas as opções de sintaxe de consulta disponíveis na análise de registos.    
