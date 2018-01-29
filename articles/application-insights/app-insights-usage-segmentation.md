---
title: "Análise de eventos, de sessão e de utilizador no Azure Application Insights | Documentos da Microsoft"
description: "Análise demográficas dos utilizadores da sua aplicação web."
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/24/2018
ms.author: mbullwin; daviste
ms.openlocfilehash: 1a5380cac08ab32cfea4cf457aed1fb1510099ed
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Análise de utilizadores, sessões e os eventos no Application Insights

Determinar quando as pessoas utilizam a aplicação web, as páginas que está a mais interessados, onde se encontram os seus utilizadores e os sistemas operativos e browsers utilizam. Analisar o negócio e telemetria de utilização através da utilização de [Azure Application Insights](app-insights-overview.md).

![Captura de ecrã de utilizadores do Application Insights](./media/app-insights-usage-segmentation/0001-users.png)

## <a name="get-started"></a>Introdução

Se não vir ainda dados a utilizadores, sessões ou painéis de eventos no portal do Application Insights, [aprender a começar a utilizar as ferramentas de utilização](app-insights-usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>A ferramenta de segmentação de utilizadores, sessões e os eventos

Três dos painéis de utilização utilizam a mesma ferramenta para dividem e organizam a telemetria da sua aplicação web de três perspetivas. Ao filtrar e dividir os dados, pode descobrir informações sobre a utilização de relativa de páginas diferentes e funcionalidades.

* **Ferramenta de utilizadores**: como muitas pessoas utilizado a aplicação e as respetivas funcionalidades.  Os utilizadores são contados utilizando IDs anónimos armazenados em cookies do browser. Uma única pessoa através de browsers diferentes ou máquinas será contada como mais do que um utilizador.
* **Ferramenta de sessões**: O número de sessões de atividade do utilizador tem incluído determinadas páginas e funcionalidades da sua aplicação. Uma sessão é contabilizada após a meia hora de inatividade do utilizador, ou após 24 horas de utilização contínua.
* **Ferramenta de eventos**: com que frequência são utilizadas determinadas páginas e funcionalidades da sua aplicação. Uma vista de página é contabilizada quando um browser carrega uma página da sua aplicação fornecida tem [instrumentadas-](app-insights-javascript.md). 

    Um evento personalizado representa uma ocorrência de algo a acontecer na aplicação, muitas vezes, uma interação do utilizador com o seguinte clique num botão ou a conclusão de algumas tarefas. Inserir o código na sua aplicação para [gerar eventos personalizados](app-insights-api-custom-events-metrics.md#trackevent).

## <a name="querying-for-certain-users"></a>Consultar determinados utilizadores

Explore a diferentes grupos de utilizadores ao ajustar as opções de consulta na parte superior da ferramenta de utilizadores:

* Mostrar: Escolha um coorte de utilizadores para analisar.
* Quem utilizada: escolha eventos personalizados e vistas de página.
* Durante a: Escolha um intervalo de tempo.
* Por: Escolha como bucket os dados, por um período de tempo ou por outra propriedade como browser ou uma localidade.
* Dividir por: Escolha uma propriedade através do qual a divisão ou segmento os dados. 
* Adicionar filtros: Limite a consulta para determinados utilizadores, sessões ou eventos com base nas respetivas propriedades, tais como o browser ou uma localidade. 
 
## <a name="saving-and-sharing-reports"></a>Guardar e partilhar relatórios 
Pode guardar relatórios de utilizadores, privados apenas para si na secção os meus relatórios ou partilhados com todas as pessoas pessoa com acesso a este recurso do Application Insights na secção dos relatórios partilhado.

Para partilhar uma hiperligação para um relatório de utilizadores, sessões ou eventos; Clique em **partilha** na barra de ferramentas, em seguida, copie a ligação.

Para partilhar uma cópia dos dados no relatório de utilizadores, sessões ou eventos; Clique em **partilha** na barra de ferramentas, em seguida, clique em de **ícone Word** para criar um documento do Word com os dados. Ou, clique em de **ícone Word** acima gráfico principal.

## <a name="meet-your-users"></a>Cumprir os seus utilizadores

O **satisfazer os seus utilizadores** secção apresenta informações sobre cinco de exemplo dos utilizadores correspondido por consulta atual. Considerar e explorar os comportamentos de indivíduos, além de agregados, podem fornecer informações sobre como as pessoas utilizam, na verdade, a sua aplicação.

## <a name="next-steps"></a>Passos Seguintes

- Para ativar as experiências de utilização, começar a enviar [eventos personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) ou [vistas de página](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Se já enviar eventos personalizados ou vistas de página, explore as ferramentas de utilização para saber como os utilizadores utilizam o serviço.
    - [Funis](usage-funnels.md)
    - [Retenção](app-insights-usage-retention.md)
    - [Fluxos do Utilizador](app-insights-usage-flows.md)
    - [Livros](app-insights-usage-workbooks.md)
    - [Adicionar o contexto de utilizador](app-insights-usage-send-user-context.md)