---
title: Compreender as práticas recomendadas do LUIS – Azure | Documentos da Microsoft
description: Aprenda as práticas recomendadas do LUIS para obter os melhores resultados.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: c1d797e3dd3a0926cfce8fee323aaaa1a866a169
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436931"
---
# <a name="best-practices"></a>Melhores práticas
Utilize a processo de criação de aplicações para criar a sua aplicação LUIS. 

* Criar modelo de idioma
* Adicionar algumas expressões de exemplo de treinamento (10 a 15 por objetivo)
* Publicar 
* Teste a partir de ponto final 
* Adicionar funcionalidades

Assim que a sua aplicação está [publicados](luis-how-to-publish-app.md), utilize o ciclo de criação de adicionar funcionalidades, publicar e teste a partir de ponto final. Não é começa o próximo ciclo de criação ao adicionar mais expressões de exemplo. Que não permitem o LUIS saiba seu modelo com expressões de utilizador do mundo real. 

Por ordem para LUIS ser eficiente em seu trabalho de aprendizagem, não expanda as expressões com até que o conjunto atual de expressões de exemplo e ponto final com retornando pontuações de predição de confiança e de alta. Melhorar as pontuações aprendizagem ativa, a utilizar [padrões](luis-concept-patterns.md), e [frase listas](luis-concept-feature.md). 

## <a name="do-and-dont"></a>Fazer e não
A lista seguinte inclui as melhores práticas para aplicações de LUIS:

|O que deve fazer|O que não deve fazer|
|--|--|
|[Definir objetivos distintos](#do-define-distinct-intents) |[Adicionar muitas expressões de exemplo para intenções](#dont-add-many-example-utterances-to-intents) |
|[Encontrar um encontramos uma boa entre demasiado genérico e muito específico para cada intenção](#do-find-sweet-spot-for-intents)|[Utilizar o LUIS como uma plataforma de treinamento](#dont-use-luis-as-a-training-platform)|
|[Criar a sua aplicação iterativamente](#do-build-the-app-iteratively)|[Adicionar muitas expressões de exemplo do formato do mesmo, a ignorar a outros formatos](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Adicionar listas de frase e padrões em iterações posteriores](#do-add-phrase-lists-and-patterns-in-later-iterations)|[Mistura a definição de objetivos e entidades](#dont-mix-the-definition-of-intents-and-entities)|
|[Adicionar expressões de exemplo para nenhum intenção](#do-add-example-utterances-to-none-intent)|[Criar listas de frase com todos os valores possíveis](#dont-create-phrase-lists-with-all-the-possible-values)|
|[Tirar partido da funcionalidade de sugerido para aprendizagem ativa](#do-leverage-the-suggest-feature-for-active-learning)|[Adicionar tantas padrões](#dont-add-many-patterns)|
|[Monitorizar o desempenho da sua aplicação](#do-monitor-the-performance-of-your-app)|[Formar e publicar com cada expressão única de exemplo adicionado](#dont-train-and-publish-with-every-single-example-utterance)|

## <a name="do-define-distinct-intents"></a>Definir objetivos distintos
Certifique-se do que vocabulário para cada intenção é apenas para esse propósito e não sobrepostos com um objetivo diferente. Por exemplo, se quiser ter uma aplicação que lidar com esquemas de viagens, tais como companhia aérea voos e hotéis, é possível tê-los como intenções separadas ou a mesma intenção com entidades de dados específicos dentro da expressão.

Se o vocabulário entre dois objetivos é o mesmo, combinar a intenção e utilizar entidades. 

Considere as expressões de exemplo seguinte:

```
Book a flight
Book a hotel
```

"Programar um vôo" e "Reservar quarto num hotel" utiliza o mesmo vocabulário de "livro um". Esta é a sobreposição de, pelo que deve ser o mesmo objetivo com as palavras diferentes de voo e hotel extraídos entidades. 

## <a name="do-find-sweet-spot-for-intents"></a>Encontrar encontramos uma boa para intenções
Utilize dados de predição do LUIS para determinar se seus objetivos estão sobrepostos. A sobreposição de objetivos confunde o LUIS. O resultado é que a parte superior a intenção de classificação é demasiado fechar para outro objetivo. Como o LUIS não utiliza o mesmo caminho exato através dos dados para cada hora de formação, um objetivo sobreposto tem uma chance de ser o primeiro ou segundo no treinamento. Quer pontuação a expressão para cada intenção de ser mais separada para que isso não acontece. Boa distinção para objetivos deve resultar na intenção de principal esperada cada vez. 
 
## <a name="do-build-the-app-iteratively"></a>Criar a aplicação de maneira iterativa
Manter um separado *cego* testar o conjunto que não é utilizado como [expressões de exemplo](luis-concept-utterance.md) ou expressões de ponto final. Continuam a melhorar a aplicação para o seu conjunto de teste. Adapte o teste definido para refletir as expressões de utilizador real. Utilize este teste cego definido como avaliar cada iteração. 

Os desenvolvedores devem ter três conjuntos de dados. A primeira é que as expressões de exemplo para criar o modelo. O segundo é para testar o modelo no ponto final. O terceiro é o blind testar dados utilizados no [testes de batch](luis-how-to-batch-test.md). Este último conjunto não é usado no treinamento da aplicação nem enviado no ponto final.  

## <a name="do-add-phrase-lists-and-patterns-in-later-iterations"></a>Adicionar listas de frase e padrões em iterações posteriores
[Frase listas](luis-concept-feature.md) permitem-lhe definir os dicionários de palavras relacionadas ao seu domínio de aplicação. Seed sua frase lista com algumas palavras, em seguida, utilizar a funcionalidade de sugerido, para que o LUIS conhece mais palavras no vocabulário. Não adicione cada palavra ao vocabulário, uma vez que a lista de expressão não é uma correspondência exata. 

Expressões de utilizador real do ponto de extremidade, muito semelhante entre si, podem revelar os padrões de escolha do word e o posicionamento. O [padrão](luis-concept-patterns.md) funcionalidade tira esta opção do word e o posicionamento, juntamente com expressões regulares para melhorar a exatidão da previsão. Uma expressão regular no padrão permite palavras e de que pretende ignorar enquanto ainda o padrão de correspondência de pontuação. 

Utilize sintaxe de opcional do padrão para pontuação, de modo pontuação pode ser ignorada.

Não se aplicam essas práticas antes da aplicação recebeu pedidos de ponto final, uma vez que o inclina a confiança.  

## <a name="do-add-example-utterances-to-none-intent"></a>A adicionar expressões de exemplo para nenhum intenção
Esta é a reversão intenção, indicado tudo fora da sua aplicação. Adicione uma expressão de exemplo para a intenção de todas as expressões de 10 exemplo no resto da sua aplicação LUIS nenhum.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>A tirar partido da funcionalidade de sugerido para aprendizagem ativa
Uso [aprendizagem ativa](label-suggested-utterances.md)da **rever expressões de ponto final** regularmente, em vez de adicionar mais expressões de exemplo para objetivos. Uma vez que a aplicação está a receber constantemente expressões de ponto final, esta lista está crescendo e alterar.

## <a name="do-monitor-the-performance-of-your-app"></a>Monitorizar o desempenho da sua aplicação
Monitorize a exatidão da previsão usando um conjunto de teste. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Não adicionar muitas expressões de exemplo para intenções
Depois da aplicação for publicada, adicione apenas expressões com aprendizagem ativa no processo iterativo. Se as expressões são muito semelhantes, adicione um padrão. 

## <a name="dont-use-luis-as-a-training-platform"></a>Não utilizar o LUIS como uma plataforma de treinamento
LUIS é específico do domínio de um modelo de idioma. Não se destina a funcionar como uma plataforma de treinamento geral. 

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

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [planear a sua aplicação](luis-how-plan-your-app.md) na sua aplicação LUIS.