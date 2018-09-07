---
title: Planear as suas aplicações de compreensão de idiomas (LUIS)
titleSuffix: Azure Cognitive Services
description: Descrever a aplicação relevante intenções e entidades e, em seguida, criar seus planos de aplicativo no Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 971d5ed47c5778315e2b7c75997c945e8489b1ce
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44052246"
---
# <a name="plan-your-luis-app"></a>Planear a sua aplicação LUIS

É importante planear a sua aplicação. Identifica o seu domínio, incluindo possíveis intenções e entidades que são relevantes para a sua aplicação.  

## <a name="identify-your-domain"></a>Identificar o seu domínio
Uma aplicação do LUIS centra-se um tópico específica do domínio.  Por exemplo, pode ter uma aplicação de viagens que executa a reserva de pedidos de suporte, voos, hotéis e carros de aluguel. Outra aplicação pode fornecer conteúdo relacionado para exercitar, os esforços de adequação de controlo e definir metas. Identificar o domínio de ajuda para encontrar palavras ou frases que são importantes para seu domínio.

> [!TIP]
> LUIS oferecem [domínios pré-concebidos](luis-how-to-use-prebuilt-domains.md) para inúmeros cenários comuns.
> Verifique se pode utilizar um domínio pré-criado como ponto de partida para a sua aplicação.

## <a name="identify-your-intents"></a>Identificar suas intenções
Pense sobre o [intenções](luis-concept-intent.md) que são importantes para tarefas de seu aplicativo. Vejamos o exemplo de uma aplicação de viagens, com as funções para programar um vôo e verificar o tempo no destino do usuário. Pode definir os objetivos de "BookFlight" e "GetWeather" para estas ações. Aplicações mais complexas com mais funções, tem mais objetivos e deve defini-los cuidadosamente modo a não ser muito específica. Por exemplo, "BookFlight" e "BookHotel" talvez precise ser intenções separadas, mas "BookInternationalFlight" e "BookDomesticFlight" podem ser muito semelhantes.

> [!NOTE]
> É melhor prática para utilizar apenas os objetivos, conforme necessário executar as funções da sua aplicação. Se definir objetivos de demasiados, torna-se mais difícil para o LUIS classificar expressões com corretamente. Se definir muito poucos ou pode ser tão gerais como para uma sobreposição.

## <a name="create-example-utterances-for-each-intent"></a>Criar expressão de exemplo para cada intenção
Depois de determinar os objetivos, crie 10 ou 15 expressões de exemplo para cada intenção. Para começar, não existem menos do que este número ou criar muitas expressões para cada intenção. Cada ocorrência de pronunciação deve ser diferente do que a expressão anterior. Uma variedade de bom nas expressões inclui contagem de palavras geral, escolha do word, tempo verbal de verbo e pontuação. 

## <a name="identify-your-entities"></a>Identificar suas entidades
As expressões de exemplo, identificar as entidades que pretende extraídos. Para programar um vôo, precisa de algumas informações como o destino, a data, a companhia aérea, a categoria de pedido de suporte e classe de viagens. Criar entidades para estes tipos de dados e, em seguida, marcar a [entidades](luis-concept-entity-types.md) nas expressões de exemplo porque eles são importantes para a realização de um objetivo. 

Quando determinar quais entidades para utilizar na sua aplicação, tenha em atenção que existem diferentes tipos de entidades para capturar as relações entre tipos de objetos. [Entidades no LUIS](luis-concept-entity-types.md) fornece mais detalhes sobre os diferentes tipos.

### <a name="simple-entity"></a>Entidade simples
Uma entidade descreve um único conceito.

![entidade Simple](./media/luis-plan-your-app/simple-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#simple-entity-data) para saber mais sobre a entidade simple a extração do ponto de extremidade de resposta da consulta JSON. Experimente isto [guia de introdução](luis-quickstart-primary-and-secondary-data.md) para saber mais sobre como utilizar uma entidade.

### <a name="hierarchical-entity"></a>Entidade hierárquica
Uma entidade hierárquica é um tipo especial de um **simples** entidade; a definição de uma categoria e seus membros na forma de relação principal-subordinado. A relação é determinada pelo contexto de dentro da expressão. Subordinados de uma entidade hierárquica também são entidades simples.

![entidades hierárquicas](./media/luis-plan-your-app/hierarchical-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#hierarchical-entity-data) para saber mais sobre a extrair a entidade hierárquica do ponto de extremidade de resposta da consulta JSON. Experimente isto [guia de introdução](luis-quickstart-intent-and-hier-entity.md) para saber mais sobre como utilizar uma entidade hierárquica.

### <a name="composite-entity"></a>Entidade composta
Uma entidade composta é constituída por outras entidades que formam as partes de um todo. Uma entidade composta contém uma variedade de tipos de entidade.

![Entidade composta](./media/luis-plan-your-app/composite-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#composite-entity-data) para saber mais sobre a extrair a entidade composta do ponto de extremidade de resposta da consulta JSON. Experimente isto [tutorial](luis-tutorial-composite-entity.md) para saber mais sobre como utilizar uma entidade composta.

### <a name="prebuilt-entity"></a>Entidade pré-criados
LUIS fornece [entidades pré-concebidas](luis-prebuilt-entities.md) para tipos de dados comuns, como o número, data, endereço de e-mail e URL. Pode utilizar a entidade pré-criados numérica para o número de pedidos de suporte numa ordem de pedido de suporte.

![Entidade pré-criados numérica](./media/luis-plan-your-app/number-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#prebuilt-entity-data) para saber mais sobre a extrair a entidade pré-criados do ponto de extremidade de resposta da consulta JSON. 

### <a name="list-entity"></a>Entidade de lista 
Uma entidade de lista é uma lista de valores explicitamente especificada. Cada valor é composta por um ou mais sinónimos. Num aplicativo de viagens, pode optar por criar uma entidade de lista para representar nomes de aeroporto.

![entidade de lista](./media/luis-plan-your-app/list-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#list-entity-data) para saber mais sobre a extração de entidades de lista do ponto de extremidade de resposta da consulta JSON. Experimente isto [guia de introdução](luis-quickstart-intent-and-list-entity.md) para saber mais sobre como utilizar uma entidade de lista.

### <a name="regular-expression-entity"></a>Entidade de expressão regular
Uma entidade de expressão regular permite que o LUIS extrair dados bem formatados de uma expressão com base numa expressão regular.

![Entidade de expressão regular](./media/luis-plan-your-app/regex-entity.png)

Ver [extração de dados](luis-concept-data-extraction.md#regular-expression-entity-data) para saber mais sobre a extração de entidades de expressão regular do ponto de extremidade de resposta da consulta JSON. Experimente o [guia de introdução](luis-quickstart-intents-regex-entity.md) para saber mais sobre como utilizar uma entidade de expressão regular.

## <a name="next-steps"></a>Passos Seguintes
Depois da aplicação é preparada, publicado e obtém as expressões de ponto de extremidade, pretendem implementar melhorias de previsão com [aprendizagem ativa](luis-how-to-review-endoint-utt.md), [frase listas](luis-concept-feature.md), e [padrões](luis-concept-patterns.md). 


* Ver [crie seu primeiro aplicativo do Language Understanding Intelligent Service (LUIS)](luis-get-started-create-app.md) para uma passo a passo rápido de como criar uma aplicação do LUIS.
