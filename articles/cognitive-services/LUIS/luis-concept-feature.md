---
title: Funcionalidades nas aplicações de LUIS nos serviços cognitivos do Azure
titleSuffix: Azure Cognitive Services
description: Adicione recursos para um modelo de idioma para fornecer sugestões sobre como a reconhecer a entrada que deseja identificar ou classificar. Recursos ajudam o LUIS reconhecer intenções e entidades.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 015679b6020e9d2a4d702f9d6e723ecd9499d8dc
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034472"
---
# <a name="phrase-list-features-in-luis"></a>Recursos de lista de frase do LUIS

No machine learning, um *funcionalidade* é uma distinção característica ou atributo de dados que observa o seu sistema. 

Adicione recursos para um modelo de idioma para fornecer sugestões sobre como a reconhecer a entrada que deseja identificar ou classificar. Recursos ajudam o LUIS reconhecer intenções e entidades, mas as funcionalidades não são objetivos ou as entidades em si. Em vez disso, recursos podem fornecer exemplos de termos relacionados.  

## <a name="what-is-a-phrase-list-feature"></a>O que é uma funcionalidade de lista de frase?
Uma lista de frase inclui um grupo de valores (palavras ou frases) que pertencem à mesma classe e deve ser tratado de maneira semelhante (por exemplo, nomes de cidades ou produtos). O que LUIS aprende sobre uma delas é aplicada automaticamente para os outros também. Esta lista não é um fechada [listar entidade](luis-concept-entity-types.md#types-of-entities) (exact correspondências de texto) de palavras correspondentes.

Uma lista de frase adiciona ao vocabulário do domínio de aplicativo como um sinal de segundo para LUIS sobre essas palavras.

## <a name="how-to-use-phrase-lists"></a>Como utilizar listas de frase
A aplicação de recursos humanos [tutorial simples de entidade](luis-quickstart-primary-and-secondary-data.md), a aplicação utiliza uma **tarefa** lista de frase de tipos de tarefa como programador, roofer e secretário. Se um dos seguintes valores o rótulo como uma entidade aprendidas por máquina, LUIS aprende a reconhecer os outros. 

Uma lista de expressão pode ser intercambiáveis ou não intercambiáveis. Uma *intercambiáveis* lista frase é para os valores que são sinónimos, e um *não intercambiáveis* lista de expressão se destina aos valores que não são sinónimos, mas ainda assim tem um sinal adicional na aplicação. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>
## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>Apresenta uma lista de frase ajuda a identificar as entidades intercambiáveis simples
Frase intercambiáveis listas são uma boa maneira de otimizar o desempenho da sua aplicação LUIS. Se a sua aplicação tiver problemas ao prever expressões com a intenção correta, ou no reconhecimento de entidades, pense se as expressões contenham palavras invulgares ou palavras que podem ser ambíguas em significado. Essas palavras são bons candidatos para incluir numa lista frase.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Apresenta uma lista de frase ajuda identificar intenções ao contexto de compreensão melhor
Uma lista de expressão não é uma instrução para o LUIS para efetuar a correspondência strict ou etiqueta sempre o todos os termos da lista de frase exatamente da mesma. É simplesmente uma dica. Por exemplo, poderia ter uma lista de expressão que indica "Patti" e "Selma" são nomes, mas LUIS ainda pode usar informações contextuais para reconhecer o que significa algo diferente de "Tornar uma reserva para 2 no cliente de Patti para o jantar" e "encontrar me orientar instruções para Selma, Geórgia". 

Adicionar uma lista de frase é uma alternativa à adição de mais expressões de exemplo para um objetivo. 

## <a name="an-interchangeable-phrase-list"></a>Uma lista de frase intercambiáveis
Utilize uma lista de frase intercambiáveis quando a lista de palavras ou fases cria uma classe ou um grupo. Um exemplo é uma lista de meses, como "Janeiro", "Fevereiro", "Março"; ou nomes como "João", "Maria", "Francisco".  Estas listas são intercambiáveis no sentido de que a expressão poderia ser rotulado com o mesmo objetivo ou entidade se uma palavra diferente na lista de frase fosse usada. Por exemplo, se "Mostrar o calendário de Janeiro" tem a mesma intenção como "Mostrar, de Fevereiro, o calendário", em seguida, as palavras que devem ser de uma lista de intercambiável. 

## <a name="a-non-interchangeable-phrase-list"></a>Uma lista de expressão não intercambiáveis
Utilize uma lista de expressão não intercambiáveis não sinônimos palavras ou expressões que podem ser agrupados em seu domínio. 

Por exemplo, utilize uma lista de expressão não intercambiáveis palavras raras, proprietárias e externa. LUIS pode não ser possível reconhecer palavras raras e proprietárias, como palavras estrangeiras (fora da cultura da aplicação). A definição não intercambiáveis indica que o conjunto de palavras raro constitui uma classe que deve aprender a reconhecer o LUIS, mas não são sinónimos ou intercambiáveis entre si.

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Quando utilizar listas de frase em relação a entidades de lista
Embora uma lista de frase e entidades de lista podem afetar os discursos em todas as intenções, cada um faz isso de forma diferente. Utilize uma lista de frase para afetar a pontuação de predição de intenção. Utilize uma entidade de lista para afetar a extração de entidades para obter uma correspondência exata de texto. 

### <a name="use-a-phrase-list"></a>Utilize uma lista de frase
Com uma lista de frase, LUIS ainda pode levar o contexto em conta e generalizar a identificar itens que são semelhantes, mas não é uma correspondência exata, como itens numa lista. Se precisar da aplicação do LUIS para poder generalizar e identificar novos itens numa categoria, utilize uma lista de frase. 

Quando quer ser capaz de reconhecer novas instâncias de uma entidade, como um agendador de reunião deve reconhecer os nomes dos novos contactos ou uma aplicação de inventário que deve reconhecer a novos produtos, utilizar outro tipo de entidade aprendidas por máquina, como um simples ou entidades hierárquicas. Em seguida, crie uma lista de frase de palavras e expressões que o ajuda a encontrar outras palavras semelhante para a entidade de LUIS. Esta lista orienta o LUIS para reconhecer exemplos da entidade através da adição de significância adicional para o valor dessas palavras. 

Listas de frase são como vocabulário específicas de domínio que o ajudam a com a melhorar a qualidade da compreensão dos objetivos e entidades. Um uso comum de uma lista de frase é nomes próprios, como os nomes das cidades. Um nome de cidade pode ter várias palavras, incluindo hífenes ou apóstrofos.
 
### <a name="dont-use-a-phrase-list"></a>Não utilize uma lista de frase 
Uma entidade de lista define explicitamente a cada valor de uma entidade pode demorar e apenas identifica os valores que correspondem exatamente. Uma entidade de lista pode ser adequada para uma aplicação em que todas as instâncias de uma entidade são conhecidas e não são alterados frequentemente. Os exemplos são itens de comida num menu de restaurante alterado com pouca frequência. Se precisar de uma correspondência exata do texto de uma entidade, não utilize uma lista de frase. 

## <a name="best-practices"></a>Melhores práticas
Saiba mais [melhores práticas](luis-concept-best-practices.md).

## <a name="next-steps"></a>Passos Seguintes

Ver [adicionar funcionalidades](luis-how-to-add-features.md) para saber mais sobre como adicionar funcionalidades à sua aplicação LUIS.
