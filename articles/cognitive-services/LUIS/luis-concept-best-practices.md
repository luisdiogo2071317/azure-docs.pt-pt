---
title: Melhores práticas
titleSuffix: Language Understanding - Azure Cognitive Services
description: Aprenda as práticas recomendadas do LUIS para obter os melhores resultados de modelo da sua aplicação LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: 5a6f9c559ce6fe66d4fe3df9382bc931f4a55e6a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55209371"
---
# <a name="best-practices-for-building-a-language-understanding-app-with-cognitive-services"></a>Melhores práticas para criar uma aplicação de compreensão de idiomas com os serviços cognitivos
Utilize a processo de criação de aplicações para criar a sua aplicação LUIS. 

* Criar modelo de idioma
* Adicionar algumas expressões de exemplo de treinamento (10 a 15 por objetivo)
* Publicar 
* Teste a partir de ponto final 
* Adicionar funcionalidades

Assim que a sua aplicação está [publicados](luis-how-to-publish-app.md), utilize o ciclo de criação de adicionar funcionalidades, publicar e teste a partir de ponto final. Não é começa o próximo ciclo de criação ao adicionar mais expressões de exemplo. Que não permitem o LUIS saiba seu modelo com expressões de utilizador do mundo real. 

Por ordem para LUIS ser eficiente em seu trabalho de aprendizagem, não expanda as expressões com até que o conjunto atual de expressões de exemplo e ponto final com retornando pontuações de predição de confiança e de alta. Melhorar as pontuações usando [aprendizagem ativa](luis-concept-review-endpoint-utterances.md), [padrões](luis-concept-patterns.md), e [frase listas](luis-concept-feature.md). 

## <a name="do-and-dont"></a>Fazer e não
A lista seguinte inclui as melhores práticas para aplicações de LUIS:

|O que deve fazer|O que não deve fazer|
|--|--|
|[Definir objetivos distintos](#do-define-distinct-intents) |[Adicionar muitas expressões de exemplo para intenções](#dont-add-many-example-utterances-to-intents) |
|[Encontrar um encontramos uma boa entre demasiado genérico e muito específico para cada intenção](#do-find-sweet-spot-for-intents)|[Utilizar o LUIS como uma plataforma de treinamento](#dont-use-luis-as-a-training-platform)|
|[Criar a sua aplicação iterativamente](#do-build-the-app-iteratively)|[Adicionar muitas expressões de exemplo do formato do mesmo, a ignorar a outros formatos](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Adicionar listas de frase e padrões em iterações posteriores](#do-add-phrase-lists-and-patterns-in-later-iterations)|[Mistura a definição de objetivos e entidades](#dont-mix-the-definition-of-intents-and-entities)|
|[Balancear sua expressões com todas as intenções](#balance-your-utterances-across-all-intents) exceto a intenção None.<br>[Adicionar expressões de exemplo para nenhum intenção](#do-add-example-utterances-to-none-intent)|[Criar listas de frase com todos os valores possíveis](#dont-create-phrase-lists-with-all-the-possible-values)|
|[Tirar partido da funcionalidade de sugerido para aprendizagem ativa](#do-leverage-the-suggest-feature-for-active-learning)|[Adicionar demasiados padrões](#dont-add-many-patterns)|
|[Monitorizar o desempenho da sua aplicação](#do-monitor-the-performance-of-your-app)|[Formar e publicar com cada expressão única de exemplo adicionado](#dont-train-and-publish-with-every-single-example-utterance)|
|[Utilize as versões para cada iteração de aplicação](#do-use-versions-for-each-app-iteration)||

## <a name="do-define-distinct-intents"></a>Definir objetivos distintos
Certifique-se do que vocabulário para cada intenção é apenas para esse propósito e não sobrepostos com um objetivo diferente. Por exemplo, se quiser ter uma aplicação que identificadores viajam disposições como companhia aérea voos e hotéis, pode optar por ter essas áreas de requerente como intenções separadas ou a mesma intenção com entidades de dados específicos dentro da expressão.

Se o vocabulário entre dois objetivos é o mesmo, combinar a intenção e utilizar entidades. 

Considere as expressões de exemplo seguinte:

|Expressões de exemplo|
|--|
|Programar um vôo|
|Reservar um hotel|

"Programar um vôo" e "Reservar quarto num hotel" utiliza o mesmo vocabulário de "livro um". Este formato é o mesmo, pelo que deve ser a mesma intenção com as palavras diferentes de voo e hotel como entidades extraídas. 

## <a name="do-find-sweet-spot-for-intents"></a>Encontrar encontramos uma boa para intenções
Utilize dados de predição do LUIS para determinar se seus objetivos estão sobrepostos. Objetivos sobrepostos confundem LUIS. O resultado é que a parte superior a intenção de classificação é demasiado fechar para outro objetivo. Como o LUIS não utiliza o mesmo caminho exato através dos dados para cada hora de formação, um objetivo sobreposto tem uma chance de ser o primeiro ou segundo no treinamento. Quer pontuação a expressão para cada intenção de ser mais separada para que este flip/flop não acontece. Boa distinção para objetivos deve resultar na intenção de principal esperada cada vez. 
 
## <a name="do-build-the-app-iteratively"></a>Criar a aplicação de maneira iterativa
Mantenha um conjunto de expressões que não é utilizado como separado [expressões de exemplo](luis-concept-utterance.md) ou expressões de ponto final. Continuam a melhorar a aplicação para o seu conjunto de teste. Adapte o teste definido para refletir as expressões de utilizador real. Utilize este teste definido como avaliar cada iteração ou a versão da aplicação. 

Os desenvolvedores devem ter três conjuntos de dados. A primeira é que as expressões de exemplo para criar o modelo. O segundo é para testar o modelo no ponto final. O terceiro é o blind testar dados utilizados no [testes de batch](luis-how-to-batch-test.md). Este último conjunto não é usado no treinamento da aplicação nem enviado no ponto final.  

## <a name="do-add-phrase-lists-and-patterns-in-later-iterations"></a>Adicionar listas de frase e padrões em iterações posteriores
[Frase listas](luis-concept-feature.md) permitem-lhe definir os dicionários de palavras relacionadas ao seu domínio de aplicação. Seed sua frase lista com algumas palavras, em seguida, utilizar a funcionalidade de sugerido, para que o LUIS conhece mais palavras no vocabulário específico para a sua aplicação. Não adicione cada palavra ao vocabulário, uma vez que a lista de expressão não é uma correspondência exata. 

Expressões de utilizador real do ponto de extremidade, muito semelhante entre si, podem revelar os padrões de escolha do word e o posicionamento. O [padrão](luis-concept-patterns.md) funcionalidade tira esta opção do word e o posicionamento, juntamente com expressões regulares para melhorar a exatidão da previsão. Uma expressão regular no padrão permite palavras e de que pretende ignorar enquanto ainda o padrão de correspondência de pontuação. 

Padrão de usar [sintaxe opcional](luis-concept-patterns.md) para pontuação, de modo pontuação pode ser ignorada. Utilize o [lista explícita](luis-concept-patterns.md#explicit-lists) para compensar problemas de sintaxe pattern.any. 

Não se aplicam essas práticas antes da aplicação recebeu pedidos de ponto final, uma vez que o inclina a confiança.  

## <a name="balance-your-utterances-across-all-intents"></a>Balancear sua expressões com todas as intenções

Por ordem para LUIS para predições para ser precisa, a quantidade de expressões de exemplo em cada intenção (exceto para None intenção), tem de ser relativamente igual. 

Se tiver um objetivo com 100 expressões de exemplo e uma intenção com 20 expressões de exemplo, a intenção de expressão de 100 terão uma taxa elevada de predição.  

## <a name="do-add-example-utterances-to-none-intent"></a>A adicionar expressões de exemplo para nenhum intenção

Este objetivo é a reversão intenção, indicado tudo fora da sua aplicação. Adicione uma expressão de exemplo para a intenção de todas as expressões de 10 exemplo no resto da sua aplicação LUIS nenhum.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>A tirar partido da funcionalidade de sugerido para aprendizagem ativa

Uso [aprendizagem ativa](luis-how-to-review-endoint-utt.md)da **rever expressões de ponto final** regularmente, em vez de adicionar mais expressões de exemplo para objetivos. Uma vez que a aplicação está a receber constantemente expressões de ponto final, esta lista está crescendo e alterar.

## <a name="do-monitor-the-performance-of-your-app"></a>Monitorizar o desempenho da sua aplicação

Monitorizar a precisão de previsão usando um [teste batch](luis-concept-batch-test.md) definido. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Não adicionar muitas expressões de exemplo para intenções

Depois da aplicação for publicada, adicione apenas expressões com aprendizagem ativa no processo iterativo. Se as expressões são muito semelhantes, adicione um padrão. 

## <a name="dont-use-luis-as-a-training-platform"></a>Não utilizar o LUIS como uma plataforma de treinamento

LUIS é específico do domínio de um modelo de idioma. Não foi criado para funcionar como uma plataforma de treinamento geral de linguagem natural. 

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>Não adicionar muitas expressões de exemplo do formato do mesmo, a ignorar a outros formatos

LUIS espera variações em expressões de com um objetivo. As expressões podem variar enquanto tem o mesmo significado geral. Variações podem incluir o comprimento da expressão, escolha do word e colocação do word. 

|Não utilize o mesmo formato|Utilize o formato variado|
|--|--|
|Comprar um pedido de suporte a Seattle<br>Comprar um pedido para Paris<br>Comprar um pedido para Orlando|Comprar 1 pedido de suporte a Seattle<br>Reservar dois lugares nos olhos vermelho para Paris seguinte segunda-feira<br>Eu gostaria de reservar 3 pedidos de suporte para Orlando de quebra de spring|

A segunda coluna utiliza diferentes verbos (comprar, reserva, livro), quantidades diferentes (1, dois, 3), e disposições diferentes palavras, mas todos têm a mesma intenção de compra de bilhetes de companhia aérea para viagem. 

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>Não misture a definição de objetivos e entidades

Crie um objetivo para qualquer ação do bot irá demorar. Utilize entidades como parâmetros que tornam essa ação possível. 

Para um chatbot que irá Reservar voos de companhia aérea, crie uma **BookFlight** intenção. Não crie um objetivo para cada companhia aérea ou de cada destino. Utilizar essas partes de dados como [entidades](luis-concept-entity-types.md) e marcá-los nas expressões de exemplo. 

## <a name="dont-create-phrase-lists-with-all-the-possible-values"></a>Não crie listas de frase com todos os valores possíveis

Fornecer exemplos de alguns a [frase listas](luis-concept-feature.md) , mas não todas as palavras. LUIS generaliza e tem o contexto em consideração. 

## <a name="dont-add-many-patterns"></a>Não adicione muitos padrões

Não adicione demasiados [padrões](luis-concept-patterns.md). LUIS destina-se para saber rapidamente com menos de exemplos. Não sobrecarregar o sistema desnecessariamente.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>Não formar e publicar com cada expressão única de exemplo

Adicione expressões de 10 ou 15 antes de treinamento e publicação. Que pode ver o impacto na precisão de previsão. Adicionar uma única expressão pode não ter um impacto visível na classificação. 

## <a name="do-use-versions-for-each-app-iteration"></a>Utilize as versões para cada iteração de aplicação

Cada ciclo de criação deve ser dentro de um novo [versão](luis-concept-version.md), clonado a partir de uma versão existente. LUIS não tem nenhum limite para as versões. Um nome de versão é utilizado como parte da rota de API, pelo que é importante escolher caracteres permitidos numa URL, bem como manter dentro a contagem de caracteres de 10 para uma versão. Desenvolva uma estratégia de nome de versão para manter as suas versões organizados. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [planear a sua aplicação](luis-how-plan-your-app.md) na sua aplicação LUIS.
