---
title: Procurar todos os recursos com o Azure Log Analytics | Documentos da Microsoft
description: Este artigo descreve como pode consultar relativamente aos recursos a partir de várias áreas de trabalho e aplicações do App Insights na sua subscrição.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 29f1e1aeb68fbd9509abfc4f9274ee6ddae89c7b
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48041582"
---
# <a name="perform-cross-resource-log-searches-in-log-analytics"></a>Efetuar pesquisas de registos de entre recursos do Log Analytics  

Anteriormente com o Azure Log Analytics, conseguia apenas analisar dados de dentro do espaço de trabalho atual e limitada a capacidade de consulta em várias áreas de trabalho definidas na sua subscrição.  Além disso, pode pesquisar apenas os itens de telemetria recolhidos a partir da sua aplicação baseada na web com o Application Insights diretamente no Application Insights ou a partir do Visual Studio.  Isso também tornou um desafio para nativamente analisar operacional e os dados das aplicações em conjunto.   

Agora, pode consultar não apenas em várias áreas de trabalho do Log Analytics, mas também dados a partir de uma aplicação específica do Application Insights no mesmo grupo de recursos, outro grupo de recursos ou outra subscrição. Isso fornece uma vista de todo o sistema dos seus dados.  Só pode realizar estes tipos de consultas [do Log Analytics](log-analytics-log-search-portals.md#log-analytics-page). O número de recursos (áreas de trabalho do Log Analytics e Application Insights aplicação) que podem ser incluídos numa única consulta está limitado a 100. 

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Consultar em áreas de trabalho do Log Analytics e o Application insights
Para fazer referência a outra área de trabalho na sua consulta, utilize o [ *área de trabalho* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) identificador e para uma aplicação do Application Insights, utilize o [ *aplicação* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())identificador.  

### <a name="identifying-workspace-resources"></a>Identificar os recursos de área de trabalho
Os exemplos seguintes demonstram consultas em áreas de trabalho do Log Analytics a devolver resumidas contagens dos registos da tabela de atualização numa área de trabalho com o nome *contosoretail-it*. 

Identificar uma área de trabalho pode ser feita de várias formas:

* Nome do recurso - é um nome legível por humanos da área de trabalho, por vezes denominado *nome do componente*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Identificar uma área de trabalho por nome de parte do princípio de exclusividade em todas as subscrições acessíveis. Se tiver várias aplicações com o nome especificado, a consulta falha devido a ambiguidade. Neste caso, tem de utilizar um dos outros identificadores.

* O nome qualificado - é o "nome completo" da área de trabalho, composto pelo nome da subscrição, o grupo de recursos e o nome do componente no seguinte formato: *subscriptionName/resourceGroup/componentName*. 

    `workspace('contoso/contosoretail/contosoretail-it').Update | count `

    >[!NOTE]
    >Uma vez que os nomes de subscrição do Azure não são exclusivos, este identificador pode ser ambígua. 
    >

* ID da área de trabalho – um ID de área de trabalho é o identificador exclusivo e imutável, atribuído a cada área de trabalho representada como um identificador exclusivo global (GUID).

    `workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update | count`

* ID do Azure de recurso – a identidade exclusiva definido pelo Azure da área de trabalho. Utilize o ID de recurso quando o nome do recurso é ambíguo.  Para áreas de trabalho, o formato é: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Áreas de trabalho/OperationalInsights/componentName*.  

    Por exemplo:
    ``` 
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Update | count
    ```

### <a name="identifying-an-application"></a>Identificar uma aplicação
Os exemplos seguintes podem devolver uma contagem resumida de pedidos efetuados em relação a uma aplicação com o nome *fabrikamapp* no Application Insights. 

Identificar uma aplicação no Application Insights pode ser feito com o *app(Identifier)* expressão.  O *identificador* argumento especifica a aplicação com um dos seguintes:

* Nome do recurso - é um nome legível por humano da aplicação, por vezes referido como o *nome do componente*.  

    `app("fabrikamapp")`

* O nome qualificado - é o "nome completo" da aplicação, composto pelo nome da subscrição, o grupo de recursos e o nome do componente no seguinte formato: *subscriptionName/resourceGroup/componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Uma vez que os nomes de subscrição do Azure não são exclusivos, este identificador pode ser ambígua. 
    >

* ID - o GUID da aplicação da aplicação.

    `app("b459b4f6-912x-46d5-9cb1-b43069212ab4").requests | count`

* ID do Azure de recurso - identidade exclusiva definido pelo Azure da aplicação. Utilize o ID de recurso quando o nome do recurso é ambíguo. O formato é: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Componentes/OperationalInsights/componentName*.  

    Por exemplo:
    ```
    app("/subscriptions/b459b4f6-912x-46d5-9cb1-b43069212ab4/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

### <a name="performing-a-query-across-multiple-resources"></a>A realização de uma consulta em vários recursos
Pode consultar vários recursos a partir de qualquer uma das suas instâncias de recursos, estes podem ser aplicações e áreas de trabalho combinadas.
    
Exemplo de consulta em duas áreas de trabalho:    
    ```
    union Update, workspace("contosoretail-it").Update, workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update
    | where TimeGenerated >= ago(1h)
    | where UpdateState == "Needed"
    | summarize dcount(Computer) by Classification
    ```

## <a name="next-steps"></a>Passos Seguintes

Reveja os [referência de pesquisa de registos do Log Analytics](https://docs.loganalytics.io/docs/Language-Reference) para ver todas as opções de sintaxe de consulta disponíveis no Log Analytics.    
