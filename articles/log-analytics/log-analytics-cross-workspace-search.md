---
title: "Executar consultas em áreas de trabalho do Log Analytics do Azure | Microsoft Docs"
description: "Este artigo descreve como pode consultar em vários áreas de trabalho e aplicações do App Insights específicas na sua subscrição."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: magoedte
ms.openlocfilehash: 403448995c28ff7172d2c3abbf3b9d67341017b4
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-perform-queries-across-multiple-log-analytics-workspaces"></a>Como executar consultas em várias áreas de trabalho de análise de registos

Anteriormente com o Log Analytics do Azure, foi apenas analisar dados a partir da área de trabalho atual e limitada a capacidade de consulta em várias áreas de trabalho definidas na sua subscrição.  

Agora, pode consultar não só em várias áreas de trabalho de análise de registos, mas também dados a partir de uma aplicação específica do Application Insights no mesmo grupo de recursos, outro grupo de recursos ou outra subscrição. Este procedimento fornece uma vista de todo sistema dos seus dados.  Este tipo de consulta só pode executar o [portal avançada](log-analytics-log-search-portals.md#advanced-analytics-portal), não no portal do Azure.  

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Consultar em áreas de trabalho de análise de registos e do Application Insights
Para fazer referência a outra área de trabalho na sua consulta, utilize o [ *área de trabalho* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) identificador e para uma aplicação do Application Insights, utilize o [ *aplicação* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())identificador.  

Por exemplo, a primeira consulta devolve resumidas contagens de atualizações necessárias pela respetiva classificação da tabela atualização tanto a partir da área de trabalho atual, outra área de trabalho com o nome *contosoretail-it*.  O segundo exemplo de consulta devolve uma contagem de pedidos efetuados em relação a uma aplicação com o nome resumida *fabrikamapp* no Application Insights. 

### <a name="identifying-workspace-resources"></a>Identificar recursos da área de trabalho
Identificar uma área de trabalho pode ser executada uma das várias formas:

* Nome de recurso - é um nome legível por humanos da área de trabalho, por vezes referido como *nome do componente*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Identificar uma área de trabalho pelo respetivo nome parte do princípio que seja exclusivo em todas as subscrições acessíveis. Se tiver várias aplicações com o nome especificado, a consulta falha devido a ambiguidade. Neste caso, tem de utilizar um do outros identificadores.

* Nome qualificado - é o "nome completo" da área de trabalho, composto pelo nome da subscrição, o grupo de recursos e o nome de componente este formato: *resourceGroup/subscriptionName/componentName*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Porque os nomes de subscrição do Azure não são exclusivos, este identificador pode ser ambígua. 
    >

* ID da área de trabalho - um ID de área de trabalho é o identificador exclusivo, imutável, atribuído a cada área de trabalho representada como um identificador exclusivo global (GUID).

    `workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count`

* Recurso ID de Azure – identidade exclusiva definido pelo Azure da área de trabalho. Utilize o ID de recurso quando o nome de recurso é ambíguo.  Para áreas de trabalho, é o formato: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Áreas de trabalho/OperationalInsights/componentName*.  

    Por exemplo:
    ``` 
    workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

### <a name="identifying-an-application"></a>Identificar uma aplicação

Identificar uma aplicação no Application Insights pode ser executada com o *app(Identifier)* expressão.  O *identificador* argumento especifica a aplicação utilizando um dos seguintes:

* Nome de recurso - é um nome legível humano da aplicação, por vezes referido como o *nome do componente*.  

    `app("fabrikamapp")`

* Nome qualificado - é o "nome completo" da aplicação e composto pelo nome da subscrição, o grupo de recursos e o nome de componente este formato: *resourceGroup/subscriptionName/componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Porque os nomes de subscrição do Azure não são exclusivos, este identificador pode ser ambígua. 
    >

* ID - o GUID da aplicação da aplicação.

    `app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count`

* Recurso ID de Azure - identidade exclusiva definido pelo Azure da aplicação. Utilize o ID de recurso quando o nome de recurso é ambíguo. O formato é: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Componentes/OperationalInsights/componentName*.  

    Por exemplo:
    ```
    app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

## <a name="next-steps"></a>Passos Seguintes

Reveja o [referência de pesquisa de registo de análise de registos](https://docs.loganalytics.io/docs/Language-Reference) para ver todas as opções de sintaxe de consulta disponíveis na análise de registos.    