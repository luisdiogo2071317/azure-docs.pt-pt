---
title: Compreender LUIS melhores práticas - Azure | Microsoft Docs
description: Saiba as LUIS melhores práticas para obter os melhores resultados.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: f68e05ba40827375fbd76448e38916c682075228
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264014"
---
# <a name="best-practices"></a>Melhores práticas
Utilize a aplicação do processo de criação para criar a sua aplicação LUIS. 

* Criar modelo de idioma
* Adicionar alguns utterances de exemplo de formação (10 a 15 por intenção)
* Publicar 
* Testar a partir do ponto final 
* Adicionar funcionalidades

Assim que a sua aplicação estiver [publicados](publishapp.md), utilize o ciclo de criação de adicionar funcionalidades, publicar e testar a partir do ponto final. Não iniciar o ciclo seguinte de criação adicionando mais utterances de exemplo. Que não lhe permita LUIS saber o seu modelo com utterances de utilizador do mundo real. 

Ordem de LUIS seja eficiente a sua tarefa do learning, não expanda os utterances até que o conjunto atual de exemplo e o ponto final utterances são devolver pontuações de predição de certeza, elevada. Melhorar as pontuações utilizando o Active Directory learning, [padrões](luis-concept-patterns.md), e [frase listas](luis-concept-feature.md). 

## <a name="do-and-dont"></a>E não
A lista seguinte inclui as melhores práticas para LUIS aplicações:

|O que deve fazer|O que não deve fazer|
|--|--|
|[Definir distintos pendentes](#do-define-distinct-intents) |[Adicionar muitos utterances de exemplo para pendentes](#dont-add-many-example-utterances-to-intents) |
|[Localizar um lugar para cima do sweet entre demasiado genérico e demasiado específicos para cada intenção](#do-find-sweet-spot-for-intents)|[Utilizar LUIS como uma plataforma de formação](#dont-use-luis-as-a-training-platform)|
|[Criar a sua aplicação iteratively](#do-build-the-app-iteratively)|[Adicionar muitos utterances de exemplo do mesmo formato, a ignorar a outros formatos](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Adicionar o frase listas e padrões de iterações posteriores](#do-add-phrase-lists-and-patterns-in-later-iterations)|[Combinação da definição de entidades e pendentes](#dont-mix-the-definition-of-intents-and-entities)|
|[Adicionar utterances de exemplo como None intenção](#do-add-example-utterances-to-none-intent)|[Criar o frase listas com todos os valores possíveis](#dont-create-phrase-lists-with-all-the-possible-values)|
|[Tirar partido da funcionalidade sugerido de aprendizagem do Active Directory](#do-leverage-the-suggest-feature-for-active-learning)|[Adicionar tantas padrões](#dont-add-many-patterns)|
|[Monitorizar o desempenho da sua aplicação](#do-monitor-the-performance-of-your-app)|[Dar formação e publicar com cada utterance único exemplo adicionado](#dont-train-and-publish-with-every-single-example-utterance)|

## <a name="do-define-distinct-intents"></a>Definir distintos pendentes
Certifique-se a vocabulário para cada intenção apenas para esse objetivo e não sobrepostas com um objetivo diferente. Por exemplo, se pretender tiverem uma aplicação que processar levar disposições como companhia aérea flights e hotéis, pode optar por ter como pendentes separados ou a mesma intenção com entidades para dados específicos dentro do utterance.

Se o vocabulário entre duas pendentes é o mesmo, combinar o objetivo e utilizam entidades. 

Considere os seguinte utterances de exemplo:

```
Book a flight
Book a hotel
```

"Livro um voo" e "Livro um átrios" utiliza o mesmo vocabulário de "livro um". Isto é sobreposição, pelo que deve ser o mesmo intenção com as palavras diferentes de trânsito e átrios extraídos entidades. 

## <a name="do-find-sweet-spot-for-intents"></a>Localizar lugar para cima do sweet para pendentes
Utilize dados de predição de LUIS para determinar se o seu pendentes são sobreposição. Sobreposição pendentes confuses LUIS. O resultado é que a parte superior da classificação de intenção é demasiado fechar para outra tentativa. Porque LUIS não utiliza o mesmo caminho exato através de dados de preparação de cada vez, o objetivo sobreposto tem uma probabilidade de ser o primeiro ou segundo na formação. Pretende Pontuar o utterance para cada intenção farther, à excepção que isto não ocorre. Boa distinção para pendentes deve resultar na intenção superior esperada sempre. 
 
## <a name="do-build-the-app-iteratively"></a>Criar a aplicação iteratively
Manter um separado *oculta* testar conjunto que não é utilizado como [utterances exemplo](luis-concept-utterance.md) ou utterances de ponto final. Manter a melhorar a aplicação para o conjunto de teste. Adapte o teste definido para refletir utterances de utilizador reais. Utilize este teste oculta definido para avaliar cada iteração. 

Os programadores devem ter três conjuntos de dados. A primeira é as utterances de exemplo para criar o modelo. A segunda serve para testar o modelo de ponto final. O terceiro é o blind testar de dados utilizadas em [batch testar](luis-how-to-batch-test.md). Neste último conjunto não é utilizado na preparação da aplicação nem enviado no ponto final.  

## <a name="do-add-phrase-lists-and-patterns-in-later-iterations"></a>Adicionar frase listas e padrões de iterações posteriores
[Frase listas](luis-concept-feature.md) permitem-lhe definir dicionários das palavras relacionados com o seu domínio de aplicação. Seed o frase lista com alguns palavras, em seguida, utilizar a funcionalidade de sugerido pelo LUIS conhece mais palavras no vocabulário. Não adicione cada palavra de vocabulário, uma vez que a lista de expressão não é uma correspondência exata. 

Utterances de utilizador reais do ponto final, muito semelhante entre si, podem revelar os padrões de opção do word e o posicionamento. O [padrão](luis-concept-patterns.md) funcionalidade tira esta opção word e o posicionamento, juntamente com expressões regulares para melhorar a exatidão da previsão. Uma expressão regular o padrão de permite palavras e de que pretende ignorar enquanto ainda o padrão de correspondência de pontuação. 

Utilize sintaxe opcional do padrão para pontuação para que pontuação pode ser ignorada.

Não aplique estas práticas antes da aplicação recebeu pedidos de ponto final porque skews que será a segurança.  

## <a name="do-add-example-utterances-to-none-intent"></a>Adicionar utterances de exemplo como None intenção
Esta é a contingência intenção, indicado tudo fora da sua aplicação. Adicione um utterance de exemplo para a intenção para cada utterances 10 exemplo o resto da sua aplicação LUIS None.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Tirar partido da funcionalidade de sugerido de aprendizagem do Active Directory
Utilize [learning Active Directory](label-suggested-utterances.md)do **rever utterances de ponto final** regularmente, em vez de adicionar mais utterances de exemplo para pendentes. Porque a aplicação está constantemente a receber utterances de ponto final, esta lista está a crescer e a alteração.

## <a name="do-monitor-the-performance-of-your-app"></a>Monitorizar o desempenho da sua aplicação
Monitorize a exatidão da previsão utilizando um conjunto de teste. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Não adicione muitos utterances de exemplo para pendentes
Depois da aplicação for publicada, adicione apenas utterances learning ativo no processo iterativo. Se forem demasiado semelhantes utterances, adicione um padrão. 

## <a name="dont-use-luis-as-a-training-platform"></a>Não utilize LUIS como uma plataforma de formação
LUIS é específico para o domínio de um modelo de linguagem. Não se destina a funcionar como uma plataforma de preparação gerais. 

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>Não adicione muitos utterances de exemplo do mesmo formato, a ignorar a outros formatos
LUIS espera variações no utterances uma intenção. Os utterances podem variar enquanto tem o mesmo significado geral. Variações podem incluir comprimento utterance, a opção de word e a colocação do word. 

|Não utilize o mesmo formato|Utilize o formato variando|
|--|--|
|Compre um pedido para Seattle<br>Compre um pedido para Paris<br>Compre um pedido para Orlando|Comprar 1 permissão para Seattle<br>Dois utilizadores individuais de reserva nos olhos vermelho Paris segunda-feira seguinte<br>Gostaria de livro 3 permissões para Orlando de quebra de mola|

A segundos coluna utiliza diferentes verbos (comprar reserva, book), quantidades diferentes (1, dois, 3), e disposições diferentes de palavras, mas todas as tem a mesma intenção dos pedidos companhia aérea para levar a compra. 

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>Não misture a definição de entidades e pendentes
Crie um objetivo para qualquer ação do bot irá demorar. Utilize as entidades como parâmetros que essa ação possíveis. 

Para chatbot que será livro companhia aérea flights, crie um **BookFlight** intenção. Não crie um objetivo para cada companhia aérea ou cada destino. Utilizar essas peças de dados como [entidades](luis-concept-entity-types.md) e marcá-los no utterances de exemplo. 

## <a name="dont-create-phrase-lists-with-all-the-possible-values"></a>Não crie o frase listas com todos os valores possíveis
Alguns exemplos de fornecer o [frase listas](luis-concept-feature.md) , mas nem todas as palavras. LUIS generaliza e tem o contexto em consideração. 

## <a name="dont-add-many-patterns"></a>Não adicione muitos padrões
Não adicione demasiados [padrões](luis-concept-patterns.md). LUIS destina-se para obter rapidamente com menos de exemplos. Não desnecessariamente a sobrecarga do sistema.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>Não dar formação e publicar com cada utterance única de exemplo
Adicione 10 ou 15 utterances antes de formação e publicação. Que permite-lhe ver o impacto na precisão de predição. Adicionar um único utterance poderá não ter um impacto visível no modelo de pontuação. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [planear a sua aplicação](plan-your-app.md) na sua aplicação LUIS.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
