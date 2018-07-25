---
title: Planear as suas aplicações de LUIS | Documentos da Microsoft
description: Descrever a aplicação relevante intenções e entidades e, em seguida, criar seus planos de aplicativo no Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: diberry
ms.openlocfilehash: 8cafe2be190988cbcbe8da616fe17c9a35c722f3
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39226028"
---
# <a name="plan-your-luis-app"></a>Planear a sua aplicação LUIS

É importante planear a sua aplicação antes de começar a criá-lo no LUIS. Prepare um contorno ou o esquema dos possíveis intenções e entidades que são relevantes para o tópico específica do domínio da sua aplicação.  

## <a name="identify-your-domain"></a>Identificar o seu domínio
Uma aplicação do LUIS centra-se um tópico específica do domínio.  Por exemplo, pode ter uma aplicação de viagens que executa a reserva de pedidos de suporte, voos, hotéis e carros de aluguel. Outra aplicação pode fornecer conteúdo relacionado para exercitar, os esforços de adequação de controlo e definir metas. 

> [!TIP]
> LUIS oferecem [domínios pré-concebidos](luis-how-to-use-prebuilt-domains.md) para inúmeros cenários comuns.
> Verifique se pode utilizar um domínio pré-criado como ponto de partida para a sua aplicação.

## <a name="identify-your-intents"></a>Identificar suas intenções
Pense sobre o [intenções](luis-concept-intent.md) que são importantes para tarefas de seu aplicativo. Vejamos o exemplo de uma aplicação de viagens, com as funções para programar um vôo e verificar o tempo no destino do usuário. Pode definir os objetivos de "BookFlight" e "GetWeather" para estas ações. Aplicações mais complexas com mais funções, tem mais objetivos e deve defini-los cuidadosamente modo a não ser muito específica. Por exemplo, "BookFlight" e "BookHotel" talvez precise ser intenções separadas, mas "BookInternationalFlight" e "BookDomesticFlight" podem ser muito semelhantes.

> [!NOTE]
> É melhor prática para utilizar apenas os objetivos, conforme necessário executar as funções da sua aplicação. Se definir objetivos de demasiados, torna-se mais difícil para o LUIS classificar expressões com corretamente. Se definir muito poucos ou pode ser tão gerais como para uma sobreposição.


## <a name="identify-your-entities"></a>Identificar suas entidades
Para programar um vôo, precisa de algumas informações como o destino, a data, a companhia aérea, a categoria de pedido de suporte e classe de viagens. Pode adicioná-las como [entidades](luis-concept-entity-types.md) porque eles são importantes para a realização de um objetivo. 

Quando determinar quais entidades para utilizar na sua aplicação, tenha em atenção que existem diferentes tipos de entidades para capturar as relações entre tipos de objetos. [Entidades no LUIS](luis-concept-entity-types.md) fornece mais detalhes sobre os diferentes tipos.

### <a name="simple-entity"></a>Entidade simples
Uma entidade descreve um único conceito.

![entidade Simple](./media/luis-plan-your-app/simple-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#simple-entity-data) para saber mais sobre a entidade simple a extração do ponto de extremidade de resposta da consulta JSON. Experimente a entidade simple [guia de introdução](luis-quickstart-primary-and-secondary-data.md) para saber mais sobre como utilizar uma entidade.

### <a name="hierarchical-entity"></a>Entidade hierárquica
Uma entidade hierárquica é um tipo especial de um **simples** entidade; a definição de uma categoria e seus membros na forma de relação principal-subordinado.

![entidades hierárquicas](./media/luis-plan-your-app/hierarchical-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#hierarchical-entity-data) para saber mais sobre a extrair a entidade hierárquica do ponto de extremidade de resposta da consulta JSON. Experimente a entidade hierárquica [guia de introdução](luis-quickstart-intent-and-hier-entity.md) para saber mais sobre como utilizar uma entidade hierárquica.

### <a name="composite-entity"></a>entidade composta
Uma entidade composta é constituída por outras entidades que formam as partes de um todo. 

![entidade composta](./media/luis-plan-your-app/composite-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#composite-entity-data) para saber mais sobre a extrair a entidade composta do ponto de extremidade de resposta da consulta JSON. Experimente a entidade composta [tutorial](luis-tutorial-composite-entity.md) para saber mais sobre como utilizar uma entidade composta.

### <a name="prebuilt-entity"></a>Entidade pré-criados
LUIS fornece [entidades pré-concebidas](luis-prebuilt-entities.md) para tipos comuns como `Number`, que pode utilizar para o número de pedidos de suporte numa ordem de pedido de suporte.

![Entidade pré-criados numérica](./media/luis-plan-your-app/number-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#prebuilt-entity-data) para saber mais sobre a extração de entidades de expressão regular do ponto de extremidade de resposta da consulta JSON. 

### <a name="list-entity"></a>Entidade de lista 
Uma entidade de lista é uma lista de valores explicitamente especificada. Cada valor é composta por um ou mais sinónimos. Num aplicativo de viagens, pode optar por criar uma entidade de lista para representar nomes de aeroporto.

![entidade de lista](./media/luis-plan-your-app/list-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#list-entity-data) para saber mais sobre a extração de entidades de lista do ponto de extremidade de resposta da consulta JSON. Experimente o [guia de introdução](luis-quickstart-intent-and-list-entity.md) para saber mais sobre como utilizar uma entidade de lista.

### <a name="regular-expression-entity"></a>Entidade de expressão regular
Uma entidade de expressão regular permite que o LUIS extrair dados de uma expressão com base numa expressão regex.

![Entidade de expressão regular](./media/luis-plan-your-app/regex-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#regular-expression-entity-data) para saber mais sobre a extração de entidades de expressão regular do ponto de extremidade de resposta da consulta JSON. Experimente o [guia de introdução](luis-quickstart-intents-regex-entity.md) para saber mais sobre como utilizar uma entidade de expressão regular.

## <a name="after-getting-endpoint-utterances"></a>Depois de obter a expressão de ponto final
Depois da aplicação obtenha expressões de ponto de extremidade, pretendem implementar melhorias de previsão com [aprendizagem ativa](luis-how-to-review-endoint-utt.md), [frase listas](luis-concept-feature.md), e [padrões](luis-concept-patterns.md). 

### <a name="patternany-entity"></a>Entidade de Pattern.any
Patterns.any é um marcador de posição de comprimento variável usado apenas numa [do padrão](luis-concept-patterns.md) expressão de modelo para marcar onde a entidade começa e termina. Expressão de modelo está em conformidade com [sintaxe correta](luis-concept-patterns.md#pattern-syntax) para identificar as entidades e ignorable texto.


## <a name="next-steps"></a>Passos Seguintes
* Ver [crie seu primeiro aplicativo do Language Understanding Intelligent Service (LUIS)](luis-get-started-create-app.md) para uma passo a passo rápido de como criar uma aplicação do LUIS.