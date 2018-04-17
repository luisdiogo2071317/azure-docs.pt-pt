---
title: Cohorts de utilização do Azure Application Insights | Documentos da Microsoft
description: Analisar conjuntos diferentes ou utilizadores, sessões, eventos ou operações que têm algo em comum
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/10/2018
ms.author: mbullwin ; daviste
ms.openlocfilehash: 68453499cc6477cc079a342906614e6873938fc8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="application-insights-cohorts"></a>Application Insights Cohorts

Um coorte é um conjunto de utilizadores, sessões, eventos ou operações que têm algo em comum. No Azure Application Insights cohorts são definidos por uma consulta de análise. Se se analisar um conjunto específico de utilizadores ou eventos repetidamente, cohorts pode dão-lhe ainda mais flexibilidade para expressar exatamente o conjunto que estiver interessado em.

![Painel cohorts](.\media\app-insights-usage-cohorts\001.png)

## <a name="cohorts-versus-basic-filters"></a>Cohorts versus filtros básicos

Enquanto cohorts são utilizadas de formas semelhantes como filtros, o facto de que a definição de um coorte é criada a partir de consultas de análise personalizadas permite que sejam muito mais adaptable e complexas. Ao contrário de filtros, pode guardar cohorts para que outros membros da sua equipa podem reutilize-as.

Pode definir um coorte de utilizadores que têm todas tentou uma nova funcionalidade na sua aplicação. Com esta coorte guardado no recurso do Application Insights, faz analisar este grupo específico de utilizadores no futuro de um clique ausente.

> [!NOTE]
> Depois de criado, cohorts estão disponíveis a partir das ferramentas de utilizadores, sessões, eventos e fluxos de utilizador.

## <a name="example-engaged-users"></a>Exemplo: utilizadores ativos

A equipa de define um utilizador acentuado como qualquer pessoa que utiliza a sua aplicação cinco ou mais vezes num determinado mês. Vamos definir um coorte destes utilizadores ativos.

1. Abra o **Cohorts** ferramenta.

2. Clique em **modelo galeria** separador. Aqui, encontrará uma coleção de modelos para vários cohorts.

3. Escolha **parte utilizadores** – por dias utilizada ".

    Existem três parâmetros para este coorte:
      * **Atividades** que permitem-lhe escolher os eventos e vistas de página devem contam como "utilização".
      * **Período de** a definição de um mês.
      * **UsedAtleastCustom** o número de vezes que precisam para utilizar algo num período de tempo para contam como um utilizador acentuado.

4. Alteração **UsedAtleastCustom** "5 + dias" e deixe **período** predefinição de 28 dias.

    ![Imagem](.\media\app-insights-usage-cohorts\003.png)

    Agora, este coorte representa todos os IDs de utilizador que foram enviados com qualquer vista de página ou eventos personalizado em cinco dias separados nos últimos 28 dias.

5. Clique em **Guardar**.

   > [!TIP]
   >  Dê um nome, a coorte como "Engaged utilizadores (dias 5 +)" e guarde-o para "Os meus relatórios" ou "Partilhado relatórios", dependendo de se pretender que outras pessoas com acesso a este recurso Appication Insights para ver este coorte.

6. Clique em **voltar a galeria**.

### <a name="what-can-you-do-with-this-cohort"></a>O que fazer com esta coorte?

Abra o **utilizadores** ferramenta > no **mostrar** pendente > Escolha coorte que criou em **os utilizadores que pertencem a...**

Agora, a ferramenta de utilizadores é filtrada para este coorte de utilizadores:

![Painel de utilizadores filtrada para um determinado coorte](.\media\app-insights-usage-cohorts\004.png)

Alguns pontos importantes a reparar:
   * Este é um conjunto que não foi possível criou através de filtros normais. A lógica de data mais avançada.
   * Pode continuar a filtrar esta coorte utilizar os filtros normais na ferramenta de utilizadores. Por isso, enquanto o coorte está definido no windows de 28 dias, pode ajustar o intervalo de tempo na ferramenta de utilizadores ser 30, 60 ou 90 dias. 

Isto permite-lhe fazer perguntas mais sofisticadas, como: _para as pessoas que foram parte nos últimos dias 28, como essas pessoas mesmas comportar-se nos últimos 60 dias?_ que de outra forma seriam impossíveis express através do construtor de consultas.

## <a name="example-events-cohort"></a>Exemplo: coorte de eventos

Também pode efetuar cohorts de eventos. Vamos definir um coorte de vistas de página de eventos e, em seguida, consulte como utilizá-los a partir de outras ferramentas. Isto poderá ser útil para definir um conjunto de eventos que considera que a sua equipa _utilização ativa_, ou para definir um conjunto de eventos relacionados com uma determinada funcionalidade nova.

1. Abra o **Cohorts** ferramenta.

2. Clique em de **modelo galeria** separador. Aqui, encontrará uma coleção de modelos para vários cohorts.

3. Escolha **selecionador de eventos**.

    ![Seleccionador de captura de ecrã de eventos](.\media\app-insights-usage-cohorts\006.png)

4. No **atividades** lista pendente, selecione os eventos que gostaria de ser de coorte

5. Guarde o coorte e atribua um nome.

## <a name="example-active-users-where-you-modify-query"></a>Exemplo: os utilizadores ativos em que modifique a consulta

Os dois cohorts anteriores foram definidos utilizando as dropdowns. Mas também é pode definir cohorts com as consultas de análises para total flexibilidade. Vamos ver como criando um coorte de utilizadores do Reino Unido.

![Imagem animada walking através da utilização da ferramenta de Cohorts](.\media\app-insights-usage-cohorts\cohorts0001.gif)

1. Abra o **Cohorts** ferramenta > clique **Galeria de modelo** separador > Escolha **coorte de utilizadores em branco**.

    ![Os utilizadores em branco coorte](.\media\app-insights-usage-cohorts\001.png)

    Existem três secções:
       * Uma secção de texto de Markdown onde possa descrever coorte mais detalhadamente para outros utilizadores na sua equipa.

       * Uma secção de parâmetros que pode utilizar para tornar os seus parâmetros, como o **atividades** e outras as dropdowns dos dois exemplos anteriores.

       * Uma secção de consulta que utilizar para definir o coorte utilizando uma consulta de análise.

    Na secção de consulta, [escrever uma consulta de análise](https://docs.loganalytics.io/index) que seleciona o determinado conjunto de linhas que descrevem o coorte pretenda definir. A ferramenta de Cohorts, em seguida, implicitamente adiciona um "| cláusula resumir por user_Id"para a consulta. Isto é pré-visualizado abaixo a consulta numa tabela, de modo, pode certificar-se a que sua consulta está a devolver resultados.

    > [!NOTE]
    > Se não vir a consulta, tente redimensionar a secção para torná-lo taller e revelar a consulta. O GIF animado no início desta secção ilustra o comportamento de redimensionamento.

2. Copiar-colar o seguinte num editor de consultas:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

3. Clique em **executar consulta**. Deverá ver os IDs de utilizador são apresentadas na tabela. Caso contrário, mude para um país onde a aplicação tem utilizadores.

4. Guarde e nome de coorte.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

_Posso definiu um coorte de utilizadores de um determinado país. Quando posso comparar este coorte na ferramenta de utilizadores para tal, definindo um filtro desse país na ferramenta de utilizadores, posso ver resultados diferentes. Porquê?_

Cohorts e os filtros são diferentes. Suponha que tem um coorte de utilizadores do Reino Unido (definido como exemplo acima) e comparar os resultados para definir o filtro "país ou região = Reino Unido."

* A versão de coorte irá mostrar todos os eventos de utilizadores que enviaram, pelo menos, um evento do Reino Unido no intervalo de tempo atual. Se dividir por país ou região, provavelmente verá vários países e regiões.
* A versão de filtros, só serão apresentados eventos a partir do Reino Unido. Enquanto que o se dividir por país ou região, apenas verá Reino Unido.

## <a name="learn-more"></a>Saiba mais
- [Idioma de consulta de análise](https://go.microsoft.com/fwlink/?linkid=856587)
- [Utilizadores, Sessões, Eventos](app-insights-usage-segmentation.md)
- [Fluxos do Utilizador](app-insights-usage-flows.md)
- [Descrição geral da utilização](app-insights-usage-overview.md)