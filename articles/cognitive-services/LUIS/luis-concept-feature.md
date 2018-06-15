---
title: Compreender as funcionalidades nas aplicações LUIS no Azure | Microsoft Docs
description: Saiba mais sobre as funcionalidades que ajudam a melhorar o desempenho de uma aplicação LUIS. As funcionalidades incluem o frase listas e padrões para reconhecer expressões regulares.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/18/2018
ms.author: v-geberr
ms.openlocfilehash: 416fadaf52d7a71dcaab81aab3bf6099213e5af5
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35356376"
---
# <a name="phrase-list-features-in-luis"></a>Funcionalidades de lista de expressão no LUIS

No machine learning, um *funcionalidade* é um trait distintivo ou um atributo de dados situa do seu sistema. 

Adicione funcionalidades ao modelo de linguagem fornecer sugestões sobre como reconhecer a entrada que pretende identificar ou classificar. Funcionalidades de ajuda LUIS reconhece pendentes e entidades, mas funcionalidades não estão pendentes ou entidades próprios. Em vez disso, as funcionalidades podem fornecer exemplos dos termos relacionados.  

## <a name="what-is-a-phrase-list-feature"></a>O que é uma funcionalidade da lista de expressão?
Uma lista de expressão inclui um grupo de valores (palavras ou frases reconhecíveis) que pertencem à mesma classe e tem de ser tratados da mesma forma (por exemplo, nomes de cidades ou produtos). O que LUIS aprende uma delas é aplicada automaticamente a outros, bem como. Não é um fechado [lista entidade](luis-concept-entity-types.md#types-of-entities) (exacta correspondências de texto) das palavras correspondentes.

Adiciona uma lista de expressão ao vocabulário do domínio de aplicação, como um sinal de segundo a LUIS sobre esses palavras.

## <a name="how-to-use-phrase-lists"></a>Como utilizar o frase listas
Numa aplicação levar agente, crie uma lista de expressão com o nome "Cidades" que contém os valores de Londres, Paris e Cairo. Se Etiquetar um destes valores como uma entidade simple num [utterance exemplo](luis-how-to-add-example-utterances.md#add-simple-entity-label) no objetivo, LUIS aprende reconhecer a outras pessoas. 

Uma lista de expressão pode ser permutáveis ou não permutáveis. Um *permutáveis* lista frase é para os valores que são sinónimos, e um *não permutáveis* lista frase destina-se de valores que não são automaticamente sinónimos, mas são semelhantes de outra forma. 

## <a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>Apresenta uma lista de expressão ajuda identificar entidades exchangeable simples
As listas de expressão exchangeable são uma boa forma de otimizar o desempenho da sua aplicação LUIS. Se a aplicação tiver problemas ao prever utterances para o objetivo correto ou reconhecer entidades, pensa sobre se o utterances contenham palavras invulgares ou palavras que poderão ser ambíguas significado. Estas palavras são ideais para incluir uma lista de expressão.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Apresenta uma lista de expressão ajuda identificar pendentes por um contexto melhor compreender
Utilize listas de expressão para palavras raras, proprietárias e externas. LUIS poderá não conseguir reconhecer palavras raras e proprietárias, bem como as palavras externas (fora o idioma da aplicação) e, por conseguinte, devem ser adicionados a uma lista de expressão. Esta lista de expressão deve ser marcada não-permutáveis, para indicar que o conjunto de palavras raros uma classe que deve saber LUIS para reconhecer de formulários, mas não são automaticamente sinónimos ou permutáveis entre si.

Uma lista de expressão não é uma instrução para LUIS para efetuar a correspondência strict ou etiqueta sempre o todos os termos de licenciamento na lista frase exatamente da mesma. É simplesmente uma sugestão. Por exemplo, pode ter uma lista de expressão que indica que "Patti" e "Selma" nomes, mas LUIS ainda pode utilizar informações contextuais para reconhecer o que significa algo diferente em "Tornar uma reserva para 2 em Diner de Patti para dinner" e "cinco-me a ocasionar instruções para Selma, Geórgia". 

A adição de uma lista de expressão é uma alternativa ao adicionar mais utterances de exemplo para o objetivo. 

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Quando utilizar o frase listas versus entidades de lista
Enquanto uma lista de expressão e entidades de lista podem ter impacto nas utterances em todos os pendentes, cada um faz isto de forma diferente. Utilize uma lista de expressão para afetar a pontuação de predição de intenção. Utilize uma entidade de lista para afetar a extração de entidade para obter uma correspondência exata de texto. 

### <a name="use-a-phrase-list"></a>Utilizar uma lista de expressão
Uma lista de expressão LUIS ainda pode ter contexto em consideração e generalizar para identificar os itens que são semelhantes, mas não é uma correspondência exata, como itens numa lista. Se precisar de aplicação LUIS conseguir generalize e identificar novos itens numa categoria, utilize uma lista de expressão. 

Quando pretender conseguir reconhecer novas instâncias de uma entidade, como um programador de reunião deve reconhecer os nomes dos novos contactos ou uma aplicação de inventário que deve reconhecer novos produtos, utilize outro tipo de entidade adquiridos por máquina, como uma simples ou entidade hierárquica. Em seguida, crie uma lista de expressão de palavras e expressões que o ajuda a LUIS encontrar outras palavras semelhantes a entidade. Esta lista orienta LUIS para reconhecer exemplos da entidade adicionando significância adicional para o valor desses palavras. 

As listas de expressão são como vocabulário específicas do domínio que o ajudar com Otimização da qualidade de compreensão das entidades e pendentes. Uma utilização comum de uma lista de expressão é substantivos adequado, como nomes de cidade. Um nome de cidade pode ter várias palavras, incluindo hífenes ou apostrophes.
 
### <a name="dont-use-a-phrase-list"></a>Não utilize uma lista de expressão 
Uma entidade de lista define explicitamente cada valor de uma entidade pode demorar e identifica apenas os valores que correspondem exatamente. Uma entidade de lista poderá ser apropriada para uma aplicação na qual todas as instâncias de uma entidade são conhecidas em não são alterados frequentemente, como os itens prato um menu de restaurantes que muda com pouca frequência. Se precisar de uma correspondência exata de texto de uma entidade, não utilize uma lista de expressão. 

## <a name="best-practices"></a>Melhores práticas
Saiba [melhores práticas](luis-concept-best-practices.md).

## <a name="next-steps"></a>Passos Seguintes

Consulte [adicionar funcionalidades](luis-how-to-add-features.md) para saber mais sobre como adicionar funcionalidades à sua aplicação LUIS.