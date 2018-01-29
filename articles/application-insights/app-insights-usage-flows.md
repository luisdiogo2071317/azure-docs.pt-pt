---
title: "Analisar padrões de navegação do utilizador com fluxos de utilizador no Azure Application Insights | Documentos da Microsoft"
description: "Analise como os utilizadores navegam entre as páginas e funcionalidades da sua aplicação web."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/24/2018
ms.author: mbullwin; daviste
ms.openlocfilehash: 63139bfb190959284cb5d25df1a8dd91bd7d913f
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Analisar padrões de navegação do utilizador com o utilizador passa no Application Insights

![Ferramenta de Insights flui de utilizador de aplicação](./media/app-insights-usage-flows/00001-flows.png)

A ferramenta de utilizador fluxos visualiza como os utilizadores navegam entre as páginas e funcionalidades do seu site. É ótimo para as respostas a questões como:

* Como os utilizadores navegam para fora de uma página no seu site?
* O que os utilizadores clicam numa página no seu site?
* Onde estão os locais que os utilizadores mais churn do seu site?
* Existem locais onde os utilizadores repetir a mesma ação mais e mais?

Inicia a ferramenta de utilizador fluxos de uma vista de página inicial, o evento personalizado ou a exceção que especificar. Tendo em conta este evento inicial, utilizador flui mostra os eventos que tenham acontecido antes e, posteriormente, durante as sessões de utilizador. Linhas de espessura variando mostram o número de vezes cada caminho foi seguido por utilizadores. Especial **iniciar sessão** nós mostram onde os nós subsequentes iniciou uma sessão. **Sessão terminado** nós mostram quantos utilizadores enviados sem vistas de página ou eventos personalizados depois do nó anterior, realçando onde os utilizadores provavelmente esquerda do site.

> [!NOTE]
> O recurso do Application Insights tem de conter vistas de página ou eventos personalizados para utilizar a ferramenta de fluxos de utilizador. [Saiba como configurar a sua aplicação para recolher as vistas de página automaticamente com o Application Insights SDK de JavaScript](app-insights-javascript.md).
>
>

## <a name="start-by-choosing-an-initial-event"></a>Comece por escolher um evento inicial

![Escolher um evento inicial para o fluxo de utilizador](./media/app-insights-usage-flows/00002-flows-initial-event.png)

Para iniciar as respostas a questões com a ferramenta de fluxos de utilizador, escolha uma vista de página inicial, o evento personalizado ou a exceção para servir o ponto de partida para a visualização:

1. Clique na ligação no **o que os utilizadores fazer após …?**  title ou clique em de **editar** botão.
2. Selecione uma vista de página, o evento personalizado ou a exceção a partir do **eventos inicial** pendente.
3. Clique em **criar gráfico**.

A coluna "Passo 1" de visualização mostra o que os utilizadores foram mais frequentemente imediatamente depois do evento inicial, ordenadas topo inferior do mais, a menos frequentes. O "Passo 2" e colunas subsequentes mostram o que os utilizadores foram depois disso, criar uma imagem de todos os utilizadores de formas tem navegar através do seu site.

Por predefinição, a ferramenta de utilizador fluxos amostras aleatoriamente apenas últimas 24 horas de vistas de página e eventos personalizados do seu site. Pode aumentar o intervalo de tempo e alterar o equilíbrio de desempenho e a precisão para a amostragem aleatório no menu de edição.

Se alguns das exceções, vistas de página e eventos personalizados não são relevantes para si, clique em de **X** em nós que pretende ocultar. Depois de selecionar os nós que pretende ocultar, clique em de **criar gráfico** no botão abaixo a visualização. Para ver todos os nós tiver oculto, clique o **editar** botão, em seguida, observar o **excluídos eventos** secção.

Se as vistas de página ou eventos personalizados estão em falta que pretende ver na visualização:

* Verifique o **excluídos eventos** secção o **editar** menu.
* Utilize os botões de adição em **outros** nós para incluir os eventos de induzirem frequentes menos na visualização.
* Se a vista de página ou eventos personalizados espera que é enviado com pouca frequência pelos utilizadores, tente aumentar o intervalo de tempo de visualização no **editar** menu.
* Certifique-se a página de ver, eventos personalizados, ou exceção que espera que esteja configurado para ser recolhidas pelo Application Insights SDK no código fonte do seu site. [Saiba mais sobre a recolha de eventos personalizados.](app-insights-api-custom-events-metrics.md)

Se pretender ver mais passos na visualização, utilize o **passos anterior** e **passos** as dropdowns acima a visualização.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Após visitando numa página ou funcionalidade, onde os utilizadores aceder e o que clicarem?

![Utilizar o fluxo de utilizador para compreender onde os utilizadores clicam](./media/app-insights-usage-flows/00003-flows-one-step.png)

Se o evento inicial é uma vista de página, a primeira coluna ("passo 1") da visualização é uma forma rápida de compreender o que os utilizadores foram imediatamente após visitando a página. Tente abrir o seu site numa janela junto a visualização de fluxos de utilizador. Compare as expectativas da forma como os utilizadores interagem com a página para a lista de eventos na coluna "Passo 1". Muitas vezes, pode ser um elemento de IU na página que parece que a sua equipa insignificant entre o mais utilizados na página. Pode ser um excelente ponto de partida para melhoramentos de design para o seu site.

Se o evento inicial é um evento personalizado, a primeira coluna mostra quais os utilizadores foram apenas depois de executar essa ação. Tal como acontece com as vistas de página, considere se o comportamento dos utilizadores observado corresponde à objetivos e as expectativas da sua equipa. Por exemplo, se o evento selecionado inicial for "Adicionar Item para compras carrinho", procure para ver se a "Aceda ao vivo" e "Concluir a compra" aparecem na visualização pouco tempo depois disso. Se o comportamento do utilizador é diferente do suas expectativas, utilize a visualização para compreender a forma como os utilizadores são obter "trapped" por predefinição atual do seu site.

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>Onde estão os locais que os utilizadores mais churn do seu site?

Observar **terminou sessão** nós que são apresentados elevado de segurança numa coluna de visualização, especialmente numa fase inicial num fluxo. Isto significa que vários utilizadores provavelmente churned do seu site depois de seguir o caminho precedente de páginas e interações de IU. Por vezes, volume de alterações esperado - depois de concluir uma compra num site de eCommerce, por exemplo - mas, normalmente, o volume de alterações é um sinal de problemas de design, fraco desempenho ou outros problemas com o site que pode ser melhorada.

Tenha em atenção que **terminou sessão** nós baseiam-se apenas na telemetria recolhida por este recurso do Application Insights. Se o Application Insights não receber a telemetria determinadas interações do utilizador, os utilizadores foi ainda tem Interagiu com o seu site dessas formas, depois da ferramenta de utilizador fluxos indica a sessão terminou.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Existem locais onde os utilizadores repetir a mesma ação mais e mais?

Procure uma vista de página ou eventos personalizados que é repetido por vários utilizadores em passos subsequentes na visualização. Normalmente, isto significa que os utilizadores estão a funcionar repetitivas ações no seu site. Se encontrar repetição, pensa sobre como alterar a estrutura do seu site ou adicionar novas funcionalidades para reduzir a repetição. Por exemplo, a adicionar a funcionalidade de edição em volume se encontrar utilizadores efetuar ações repetitivas em cada linha de um elemento de tabela.

## <a name="common-questions"></a>Perguntas comuns

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>Não representam o evento inicial na primeira vez que o evento é apresentado numa sessão, nem sempre é apresentado numa sessão?

O evento inicial na visualização representa apenas a primeira vez que um utilizador enviados nessa vista de página ou eventos personalizados durante uma sessão. Se os utilizadores podem enviar o evento inicial várias vezes numa sessão, em seguida, a coluna "Passo 1" apenas mostra como os utilizadores comportar-se após o *primeiro* instância de evento inicial, nem todas as instâncias.

### <a name="some-of-the-nodes-in-my-visualization-are-too-high-level-for-example-a-node-that-just-says-button-clicked-how-can-i-break-it-down-into-more-detailed-nodes"></a>Alguns de nós no meu visualização são demasiado alto nível. Por exemplo, um nó que apenas diz "Botão Clicked." Como pode posso dividi-lo em nós mais detalhadas?

Utilize o **dividir por** opções no **editar** menu:

1. Escolha o evento que pretende dividir **eventos** menu.
2. Escolha uma dimensão no **dimensão** menu. Por exemplo, se tiver um evento chamado "Clicar no botão", experimente uma propriedade personalizada denominada "Nome do botão".

## <a name="next-steps"></a>Passos Seguintes

* [Descrição geral da utilização](app-insights-usage-overview.md)
* [Os utilizadores, sessões e os eventos](app-insights-usage-segmentation.md)
* [Retenção](app-insights-usage-retention.md)
* [Adicionar eventos personalizados para a sua aplicação](app-insights-api-custom-events-metrics.md)