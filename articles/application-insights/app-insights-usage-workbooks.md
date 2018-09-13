---
title: Investigar e partilhar dados de utilização com livros interativos no Application Insights do Azure | Documentos da Microsoft
description: Análise demográficas de utilizadores da sua aplicação web.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 06/12/2017
ms.reviewer: daviste
ms.author: mbullwin
ms.openlocfilehash: 016a26acc153fba1c38d926fd5389d02755c2ff5
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35648970"
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a>Investigar e partilhar dados de utilização com livros interativos no Application Insights

Pastas de trabalho combinam [do Azure Application Insights](app-insights-overview.md) visualizações de dados, [consultas de análise](app-insights-analytics.md)e o texto em documentos interativos. Pastas de trabalho são editáveis por outros membros da Equipe com o acesso ao mesmo recurso do Azure. Isso significa que as consultas e os controlos utilizados para criar uma pasta de trabalho estão disponíveis para outras pessoas ler a pasta de trabalho, tornando-as fáceis de explorar, estender e verificar a existência de erros.

Pastas de trabalho são úteis para cenários como:

* Explorar a utilização da sua aplicação, quando não sabe de antemão as métricas de interesse: número de utilizadores, taxas de retenção, as taxas de conversão, etc. Ao contrário de outras ferramentas de análise de utilização no Application Insights, as pastas de trabalho permitem-lhe combinar vários tipos de visualizações e análises, tornando-os excelentes para esse tipo de exploração de forma livre.
* Para a sua equipa, que explica como um recurso recém-lançada está sendo executada, por utilizador que mostra contagens para interações de chave e outras métricas.
* Compartilhamento dos resultados de um A / B de experimentação na sua aplicação com outros membros da sua equipa. Pode explicar os objetivos para a experimentação com o texto e depois mostrar cada métrica de utilização e a consulta do Analytics utilizados para avaliar a experimentação, juntamente com claras explicativos para se cada uma foi acima ou abaixo-soltar.
* O impacto de uma indisponibilidade de relatórios sobre a utilização da sua aplicação, a combinação de dados, a explicação de texto e uma discussão das próximas etapas para evitar interrupções no futuro.

> [!NOTE]
> Recurso do Application Insights pode conter vistas de página ou eventos personalizados para usar pastas de trabalho. [Saiba como configurar a sua aplicação para recolher as vistas de página automaticamente com o Application Insights JavaScript SDK](app-insights-javascript.md).
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Editar, reorganizar, a clonagem e excluir seções do livro

Uma pasta de trabalho é um feitas das seções: visualizações de utilização de forma independente editável, gráficos, tabelas, texto ou dos resultados da consulta de análise.

Para editar o conteúdo de uma seção de pasta de trabalho, clique nas **editar** botão abaixo e à direita da secção de pasta de trabalho.

![Controles de edição de secção de pastas de trabalho do Application Insights](./media/app-insights-usage-workbooks/editing-controls.png)

1. Quando terminar uma seção de edição, clique em **feito editando** no canto inferior esquerdo da secção.

2. Para criar um duplicado de uma seção, clique a **clonar esta secção** ícone. A criação de secções duplicadas é uma ótima maneira de iterar numa consulta sem perder iterações anteriores.

3. Para mover para cima uma seção numa pasta de trabalho, clique nas **mover para cima** ou **mover para baixo** ícone.

4. Para remover permanentemente uma secção, clique a **remover** ícone.

## <a name="adding-usage-data-visualization-sections"></a>Adicionar secções de visualização de dados de utilização

Pastas de trabalho oferecem quatro tipos de visualizações de análise de utilização interna. Cada responde a uma pergunta comum sobre a utilização da sua aplicação. Para adicionar tabelas e gráficos que não seja estas quatro seções, adicione secções de consulta do Analytics (ver abaixo).

Para adicionar utilizadores, sessões, eventos ou retenção secção ao seu livro, utilize o **adicionar usuários** ou outro botão correspondente na parte inferior da pasta de trabalho ou na parte inferior de qualquer seção.

![Secção de utilizadores de pastas de trabalho](./media/app-insights-usage-workbooks/users-section.png)

**Os utilizadores** secções responder "quantos utilizadores vistos alguns página ou utilizado algum recurso do meu site?"

**Sessões** secções responder "quantas sessões os usuários passam a ver algumas página ou usando algum recurso do meu site?"

**Eventos** secções responder "quantas vezes aos utilizadores ver alguns página ou usar algum recurso do meu site?"

Cada um desses tipos de três secção oferece os mesmos conjuntos de controles e visualizações:

* [Saiba mais sobre a edição de secções de utilizadores, sessões e eventos](app-insights-usage-segmentation.md)
* Ativar/desativar o gráfico principal, grades de histograma, insights automática e visualizações de utilizadores de exemplo com o **Mostrar gráfico**, **Mostrar grelha**, **Mostrar Insights**e **Exemplo destes utilizadores** caixas de verificação na parte superior de cada secção.

![Secção de retenção de pastas de trabalho](./media/app-insights-usage-workbooks/retention-section.png)

**Retenção** secções responder "De pessoas que visualizada alguns página ou utilizada algum recurso num dia ou semana, quantos voltaram numa semana ou dia subsequente?"

* [Saiba mais sobre a edição de secções de retenção](app-insights-usage-retention.md)
* Ativar/desativar a utilização de gráfico de retenção geral opcional a **gráfico de retenções totais Show** caixa de verificação na parte superior da seção.

## <a name="adding-application-insights-analytics-sections"></a>Adicionar o Application Insights Analytics secções

![Secção de análise de pastas de trabalho](./media/app-insights-usage-workbooks/analytics-section.png)

Para adicionar uma secção de consulta do Analytics do Application Insights ao seu livro, utilize o **consulta do Analytics adicionar** botão na parte inferior da pasta de trabalho ou na parte inferior de qualquer seção.

Secções de consulta do Analytics permitem-lhe adicionar arbitrárias consultas sobre os seus dados do Application Insights para pastas de trabalho. Esta flexibilidade significa secções de consulta do Analytics devem ser o go-to para responder a quaisquer perguntas sobre o site que não sejam os quatro listados acima para os utilizadores, sessões, eventos e a retenção, como:

* Como muitas exceções seu site colocado durante o período de tempo mesmo como um declínio em utilização?
* Qual era a distribuição de tempos de carregamento de página para utilizadores que visualizam alguns página?
* O número de utilizadores visualizados um conjunto de páginas em seu site, mas não de algum outro conjunto de páginas? Isso pode ser útil para entender se tiver clusters de utilizadores que utilizam diferentes subconjuntos de funcionalidade de seu site (utilizar o `join` operador com o `kind=leftanti` modificador na linguagem de consulta do Log Analytics).

Utilize o [referência de linguagem de consulta do Log Analytics](https://docs.loganalytics.io/) para saber mais sobre como escrever consultas.

## <a name="adding-text-and-markdown-sections"></a>Adicionar texto e secções do Markdown

Adicionar títulos, explicações e comentários para os livros o ajuda a transformar um conjunto de tabelas e gráficos numa narrativa. Secções de texto no suporte de pastas de trabalho do [sintaxe de Markdown](https://daringfireball.net/projects/markdown/) para formatação de texto, como cabeçalhos, negrito, itálico e listas com marcas.

Para adicionar uma secção de texto ao seu livro, utilize o **adicionar texto** botão na parte inferior da pasta de trabalho ou na parte inferior de qualquer seção.

## <a name="saving-and-sharing-workbooks-with-your-team"></a>A guardar e partilhar pastas de trabalho com a sua equipa

Pastas de trabalho são salvas dentro de um recurso do Application Insights, no **os meus relatórios** secção é privada para ou na **relatórios partilhados** secção que esteja acessível para todos os utilizadores com acesso à Recurso do Application Insights. Para ver todas as pastas de trabalho no recurso, clique a **aberto** botão na barra de ação.

Para partilhar uma pasta de trabalho que se encontra atualmente num **os meus relatórios**:

1. Clique em **aberto** na barra de ação
2. Clique no botão "..." ao lado da pasta de trabalho que pretende partilhar
3. Clique em **mover para relatórios partilhados**.

Para partilhar uma pasta de trabalho com uma ligação ou através de e-mail, clique em **partilhar** na barra de ação. Tenha em atenção que os destinatários da ligação precisam de acesso a este recurso no portal do Azure para ver a pasta de trabalho. Para fazer edições, os destinatários precisam, pelo menos, permissões de Contribuidor para o recurso.

Para afixar uma ligação para uma pasta de trabalho a um Dashboard do Azure:

1. Clique em **aberto** na barra de ação
2. Clique no botão "..." ao lado da pasta de trabalho que pretende afixar
3. Clique em **afixar ao dashboard**.

## <a name="next-steps"></a>Passos Seguintes

## <a name="next-steps"></a>Passos Seguintes
- Para permitir experiências de utilização, começar a enviar [eventos personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) ou [vistas de página](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Se já tiver enviar eventos personalizados ou vistas de página, explore as ferramentas de utilização para saber como os utilizadores utilizam o seu serviço.
    - [Utilizadores, Sessões, Eventos](app-insights-usage-segmentation.md)
    - [Funis](usage-funnels.md)
    - [Retenção](app-insights-usage-retention.md)
    - [Fluxos do Utilizador](app-insights-usage-flows.md)
    - [Adicionar contexto de utilizador](app-insights-usage-send-user-context.md)
    
