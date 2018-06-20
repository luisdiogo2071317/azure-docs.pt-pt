---
title: Planear as suas aplicações LUIS | Microsoft Docs
description: Descrevem as entidades e pendentes de aplicação relevante e, em seguida, criar os planos de aplicação no idioma compreender inteligente serviços (LUIS).
services: cognitive-services
author: DeniseMak
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: 7aec5d5b90ac7145ce9f337ec74c590b4b88c6b1
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266367"
---
# <a name="plan-your-luis-app"></a>Planear a sua aplicação LUIS

É importante planear a sua aplicação antes de começar a criar no LUIS. Prepare uma contorno ou um esquema pendentes possíveis e entidades que são relevantes para o tópico específicas do domínio da sua aplicação.  

## <a name="identify-your-domain"></a>Identificar o seu domínio
Uma aplicação LUIS centra-se um tópico específicas do domínio.  Por exemplo, pode ter uma aplicação de levar a que efetua booking de pedidos de suporte, flights, hotéis e carros rental. Outra aplicação pode fornecer conteúdo relacionados com exercising, ao controlar esforços adequação e definir objetivos. 

> [!TIP]
> Oferece LUIS [domínios prebuilt](luis-how-to-use-prebuilt-domains.md) para muitos cenários comuns.
> Verifique se pode utilizar um domínio prebuilt como um ponto de partida para a sua aplicação.

## <a name="identify-your-intents"></a>Identificar a sua pendentes
Considere sobre o [pendentes](luis-concept-intent.md) que são importantes para tarefas da sua aplicação. Vamos o exemplo de uma aplicação de levar, com as funções para o livro um voo e verifique a meteorologia no destino do utilizador. Pode definir os pendentes "BookFlight" e "GetWeather" para estas ações. Numa aplicação mais complexa com mais de funções, tem mais pendentes e deve defini-los cuidadosamente, de modo a não ser demasiado específico. Por exemplo, "BookFlight" e "BookHotel" poderão ter de ser separados pendentes, mas "BookInternationalFlight" e "BookDomesticFlight" podem ser demasiado semelhantes.

> [!NOTE]
> É uma melhor prática para utilizar apenas pendentes tantos conforme necessário executar as funções da sua aplicação. Se definir demasiados pendentes, torna mais difícil LUIS classificar utterances corretamente. Se definir demasiado alguns, poderá ser, por isso, gerais relativamente a sobreposição.


## <a name="identify-your-entities"></a>Identificar as entidades
Para o livro um voo, precisa de algumas informações como o destino, data, companhia aérea, categoria de permissão e viajam classe. Pode adicionar estes como [entidades](luis-concept-entity-types.md) porque são importantes para os realizar objetivo. 

Quando determinar que entidades para utilizar na sua aplicação, tenha em atenção que existem diferentes tipos de entidades para capturar as relações entre tipos de objetos. [As entidades de LUIS](luis-concept-entity-types.md) fornece mais detalhes sobre os diferentes tipos.

### <a name="simple-entity"></a>Entidade Simple
Uma entidade simple descreve um único conceito.

![entidade Simple](./media/luis-plan-your-app/simple-entity.png)

Consulte [extração de dados](luis-concept-data-extraction.md#simple-entity-data) para saber mais sobre a extrair a entidade simple do ponto final de uma resposta à consulta JSON. Tente a entidade simple [início rápido](luis-quickstart-primary-and-secondary-data.md) para obter mais informações sobre como utilizar uma entidade simple.

### <a name="hierarchical-entity"></a>Entidade hierárquica
Uma entidade hierárquica é um tipo especial de um **simples** entidade; definir uma categoria e os seus membros sob a forma de relação principal-subordinado.

![entidade hierárquica](./media/luis-plan-your-app/hierarchical-entity.png)

Consulte [extração de dados](luis-concept-data-extraction.md#hierarchical-entity-data) para saber mais sobre a extrair a entidade hierárquica do ponto final de uma resposta à consulta JSON. Tente a entidade hierárquica [início rápido](luis-quickstart-intent-and-hier-entity.md) para obter mais informações sobre como utilizar uma entidade hierárquica.

### <a name="composite-entity"></a>Entidade composta
Uma entidade composta é constituída por outras entidades que formam partes de um todo. 

![entidade composta](./media/luis-plan-your-app/composite-entity.png)

Consulte [extração de dados](luis-concept-data-extraction.md#composite-entity-data) para saber mais sobre a extrair a entidade composta do ponto final de uma resposta à consulta JSON. Tente a entidade composta [tutorial](luis-tutorial-composite-entity.md) para obter mais informações sobre como utilizar uma entidade composta.

### <a name="prebuilt-entity"></a>Entidade prebuilt
Fornece LUIS [entidades prebuilt](Pre-builtEntities.md) para tipos comuns como `Number`, que pode utilizar para o número de pedidos por uma ordem de permissão.

![Entidade prebuilt número](./media/luis-plan-your-app/number-entity.png)

Consulte [extração de dados](luis-concept-data-extraction.md#prebuilt-entity-data) para saber mais sobre a extração de entidades de expressão regular do ponto final resposta à consulta JSON. 

### <a name="list-entity"></a>Entidade de lista 
Uma entidade de lista é uma lista de valores explicitamente especificada. Cada valor é composta por um ou mais sinónimos. Numa aplicação levar, pode optar por criar uma entidade de lista para representar os nomes de airport.

![entidade de lista](./media/luis-plan-your-app/list-entity.png)

Consulte [extração de dados](luis-concept-data-extraction.md#list-entity-data) para saber mais sobre a extração de entidades de lista o ponto final de uma resposta à consulta JSON. Repita o [início rápido](luis-quickstart-intent-and-list-entity.md) para obter mais informações sobre como utilizar uma entidade de lista.

### <a name="regular-expression-entity"></a>Entidade de expressão regular
Uma entidade de expressão regular permite LUIS extrair dados um utterance baseado numa expressão regex.

![Entidade de expressão regular](./media/luis-plan-your-app/regex-entity.png)

Consulte [extração de dados](luis-concept-data-extraction.md#regular-expression-entity-data) para saber mais sobre a extração de entidades de expressão regular do ponto final resposta à consulta JSON. Repita o [início rápido](luis-quickstart-intents-regex-entity.md) para obter mais informações sobre como utilizar uma entidade de expressão regular.

## <a name="after-getting-endpoint-utterances"></a>Após obter utterances de ponto final
Depois da aplicação obtém utterances de ponto final, se pretende implementar melhoramentos de predição com [learning Active Directory](label-suggested-utterances.md), [frase listas](luis-concept-feature.md), e [padrões](luis-concept-patterns.md). 

### <a name="patternany-entity"></a>Entidade Pattern.any
Patterns.any é um marcador de posição de comprimento variável utilizada apenas por um [do padrão](luis-concept-patterns.md) utterance de modelo para marcar em que a entidade inicia e termina. Utterances modelo está em conformidade com [a sintaxe correta](luis-concept-patterns.md#pattern-syntax) para identificar as entidades e texto ignorable.


## <a name="next-steps"></a>Passos Seguintes
* Consulte [criar a sua primeira aplicação de idioma compreender inteligente serviços (LUIS)] [ luis-get-started-create-app] para obter instruções sobre como criar uma aplicação LUIS rápida.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app