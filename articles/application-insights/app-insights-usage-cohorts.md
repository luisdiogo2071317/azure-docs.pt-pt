---
title: Cohorts de utilização do Azure Application Insights | Microsoft Docs
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
ms.openlocfilehash: f8d566f552c86f749b914ffed70512209ad76ab7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="application-insights-cohorts"></a>Cohorts do Application Insights

Um coorte é um conjunto de utilizadores, sessões, eventos ou operações que têm algo em comum. No Azure Application Insights, cohorts são definidos por uma consulta de análise. Nos casos em que tem de analisar um específico definido de utilizadores ou eventos repetidamente, cohorts pode dão-lhe uma maior flexibilidade para express exatamente o conjunto que estiver interessado em.

![Painel cohorts](.\media\app-insights-usage-cohorts\001.png)

## <a name="cohorts-versus-basic-filters"></a>Cohorts versus filtros básicos

São utilizados cohorts formas semelhante para os filtros. Mas as definições dos cohorts são criadas de consultas de análise personalizada, pelo que são muito mais adaptable e complexas. Ao contrário de filtros, pode guardar cohorts para que outros membros da sua equipa podem reutilize-as.

Pode definir um coorte de utilizadores que têm todas tentou uma nova funcionalidade na sua aplicação. Pode guardar esta coorte num recurso do Application Insights. É fácil analisar este grupo de utilizadores específicos guardado no futuro.

> [!NOTE]
> Depois de são criados, cohorts estão disponíveis nas ferramentas de utilizadores, sessões, eventos e fluxos de utilizador.

## <a name="example-engaged-users"></a>Exemplo: Utilizadores ativos

A equipa de define um utilizador acentuado como qualquer pessoa que utiliza a sua aplicação cinco ou mais vezes num determinado mês. Nesta secção, é possível definir um coorte destes utilizadores ativos.

1. Abra a ferramenta de Cohorts.

2. Selecione o **modelo galeria** separador. Consulte uma coleção de modelos para vários cohorts.

3. Selecione **acentuados utilizadores – por dias utilizaram**.

    Existem três parâmetros para este coorte:
    * **Atividades**, onde escolher os eventos e vistas de página contam como "utilização".
    * **Período de**, a definição de um mês.
    * **UsedAtleastCustom**, o número de vezes que os utilizadores têm de utilizar algo num período de tempo para contabilizar como parte.

4. Alteração **UsedAtleastCustom** para **5 + dias**e deixe **período** a predefinição de 28 dias.

    ![Utilizadores ativos](.\media\app-insights-usage-cohorts\003.png)

    Agora este coorte representa todos os IDs de utilizador enviados com qualquer vista de página ou eventos personalizada 5 separar dias no passado 28.

5. Selecione **Guardar**.

   > [!TIP]
   >  Dê um nome, a coorte como "Engaged utilizadores (5 + dias)." Guarde-o para "Os meus relatórios" ou "Partilhado relatórios," consoante pretenda outras pessoas que têm acesso a este recurso do Application Insights para ver este coorte.

6. Selecione **voltar a galeria**.

### <a name="what-can-you-do-by-using-this-cohort"></a>O que pode fazer utilizando este coorte?

Abra a ferramenta de utilizadores. No **mostrar** pendente caixa, escolha o coorte que criou em **os utilizadores que pertencem ao**.

Agora, a ferramenta de utilizadores é filtrada para este coorte de utilizadores:

![Painel de utilizadores filtrada para um determinado coorte](.\media\app-insights-usage-cohorts\004.png)

Alguns pontos importantes a reparar:
* Não é possível criar este conjunto através de filtros normais. A lógica de data mais avançada.
* Pode continuar a filtrar esta coorte ao utilizar os filtros normais na ferramenta de utilizadores. Por isso, embora o coorte está definido no windows de 28 dias, pode ajustar o intervalo de tempo na ferramenta de utilizadores ser 30, 60 ou 90 dias.

Estes filtros suportam perguntas mais sofisticadas impossíveis express através do construtor de consultas. Um exemplo é _pessoas que foram parte nos últimos 28 dias. Como essas pessoas mesmas comportar-se nos últimos 60 dias?_

## <a name="example-events-cohort"></a>Exemplo: Coorte de eventos

Também pode efetuar cohorts de eventos. Nesta secção, é possível definir um coorte dos eventos e vistas de página. Em seguida, verá como utilizá-los a partir de outras ferramentas. Este coorte poderá definir um conjunto de eventos que considera que a sua equipa _utilização ativa_ ou um conjunto relacionados com uma determinada funcionalidade nova.

1. Abra a ferramenta de Cohorts.

2. Selecione o **modelo galeria** separador. Verá uma coleção de modelos para vários cohorts.

3. Selecione **selecionador de eventos**.

    ![Captura de ecrã do Seletor de eventos](.\media\app-insights-usage-cohorts\006.png)

4. No **atividades** pendente caixa, selecione os eventos que pretende ter a coorte.

5. Guarde o coorte e atribua um nome.

## <a name="example-active-users-where-you-modify-a-query"></a>Exemplo: Os utilizadores ativos onde modificar uma consulta

Os dois cohorts anteriores foram definidos utilizando caixas de lista pendente. Mas também pode definir cohorts através de consultas de análises para total flexibilidade. Para ver como, criar um coorte de utilizadores do Reino Unido.

![Imagem animada walking através da utilização da ferramenta de Cohorts](.\media\app-insights-usage-cohorts\cohorts0001.gif)

1. Abrir a ferramenta de Cohorts, selecione o **modelo galeria** separador e selecione **coorte de utilizadores em branco**.

    ![Coorte de utilizadores em branco](.\media\app-insights-usage-cohorts\001.png)

    Existem três secções:
    * Uma Markdown texto secção, onde o utilizador descreve coorte mais detalhadamente para outros utilizadores na sua equipa.

    * Uma secção de parâmetros, onde efetuar os seus parâmetros, como **atividades** e outras caixas de lista pendente dos dois exemplos anteriores.

    * Uma secção de consulta, onde é possível definir o coorte utilizando uma consulta de análise.

    Na secção de consulta, [escrever uma consulta de análise](https://docs.loganalytics.io/index). A consulta seleciona determinado conjunto de linhas que descrevem o coorte que pretenda definir. A ferramenta de Cohorts, em seguida, implicitamente adiciona um "| cláusula resumir por user_Id"para a consulta. Estes dados estão pré-visualizado abaixo a consulta numa tabela, pelo que, pode certificar-se a que sua consulta está a devolver resultados.

    > [!NOTE]
    > Se não vir a consulta, tente redimensionar a secção para torná-lo taller e revelar a consulta. O GIF animado no início desta secção ilustra o comportamento de redimensionamento.

2. Copie e cole o seguinte texto do editor de consultas:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

3. Selecione **executar consulta**. Se não vir IDs de utilizador são apresentadas na tabela, altere para um país onde a aplicação tem utilizadores.

4. Guarde e nome de coorte.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

_Posso definiu um coorte de utilizadores de um determinado país. Quando posso comparar este coorte na ferramenta de utilizadores para tal, definindo um filtro desse país, posso ver resultados diferentes. Porquê?_

Cohorts e os filtros são diferentes. Suponha que tem um coorte de utilizadores do Reino Unido (definido como o exemplo anterior) e comparar os resultados para definir o filtro "país ou região = Reino Unido."

* A versão de coorte mostra todos os eventos de utilizadores que enviou um ou mais eventos a partir do Reino Unido no intervalo de tempo atual. Se dividir por país ou região, provavelmente, consulte vários países e regiões.
* A versão de filtros mostra apenas os eventos a partir do Reino Unido. Mas se dividir por país ou região, verá apenas o Reino Unido.

## <a name="learn-more"></a>Saiba mais
- [Idioma de consulta de análise](https://go.microsoft.com/fwlink/?linkid=856587)
- [Os utilizadores, sessões, eventos](app-insights-usage-segmentation.md)
- [Fluxos de utilizador](app-insights-usage-flows.md)
- [Descrição geral da utilização](app-insights-usage-overview.md)