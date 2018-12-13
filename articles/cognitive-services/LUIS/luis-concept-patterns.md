---
title: Padrões de ajudar a predição
titleSuffix: Language Understanding - Azure Cognitive Services
description: Um padrão permite-lhe obter maior exatidão para um objetivo sem fornecer expressões de com muitos mais.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: diberry
ms.openlocfilehash: 46c9eb99d808874e0f49dee5fa4865a4867873f1
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271526"
---
# <a name="patterns-improve-prediction-accuracy"></a>Padrões de melhorar a exatidão da previsão
Padrões foram criados para melhorar a precisão quando várias expressões são muito semelhantes.  Um padrão permite-lhe obter maior exatidão para um objetivo sem fornecer expressões de com muitos mais. 

## <a name="patterns-solve-low-intent-confidence"></a>Padrões de resolver a confiança de intenção baixa
Considere uma aplicação de recursos humanos que os relatórios no organograma em relação a um funcionário. Dado o nome e a relação de um funcionário, LUIS retorne os funcionários envolvidos. Considere um funcionário, Tom, com um Gestor de Alice de nome e uma equipe de subordinados com o nome: Michael, Rebecca e Carl.

![Imagem de gráfico de organização](./media/luis-concept-patterns/org-chart.png)

|Expressões|Intenção prevista|Pontuação de intenção|
|--|--|--|
|Quem é subordinada de José?|GetOrgChart|.30|
|Quem é o subordinado de Tom?|GetOrgChart|.30|

Se tiver uma aplicação entre 10 e 20 expressões com diferentes comprimentos de sentença, ordem das palavras diferente e até mesmo palavras (sinónimos de "subordinado", "Gerir", "relatório"), o LUIS pode devolver uma pontuação de confiança baixa. Para ajudar a compreender a importância da ordem das palavras do LUIS, crie um padrão. 

Padrões de resolver as seguintes situações: 

* Quando a classificação de intenção é baixa
* Quando a intenção correta não é a classificação superior, mas muito próximo a classificação superior. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Padrões não são uma garantia de intenção
Padrões de utilizam uma combinação de tecnologias de predição. A definição de um objetivo de uma expressão de modelo num padrão não é uma garantia de predição intenção, mas é um sinal forte. 

## <a name="patterns-do-not-improve-entity-detection"></a>Padrões não aperfeiçoar a detecção de entidade
Embora padrões precisam entidades, um padrão não ajuda a detetar a entidade. Um padrão destina-se apenas para ajudar a predição com intenções e funções.  

Não espere ver a predição de entidade melhorado se fechar várias expressões de com num padrão único. Para obter entidades simples acionar, terá de adicionar expressões ou utilize entidades de lista mais que seu padrão de não serão acionados.

## <a name="patterns-use-entity-roles"></a>Padrões de utilizam as funções de entidade
Se dois ou mais entidades num padrão contextualmente estejam relacionadas, padrões de utilizam entidade [funções](luis-concept-roles.md) para extrair informações contextuais sobre entidades. Isso é equivalente a crianças de entidades hierárquicas, mas é **apenas** disponíveis em padrões. 

## <a name="prediction-scores-with-and-without-patterns"></a>Pontuações de previsão com e sem padrões
Tendo em conta suficiente expressões de exemplo, LUIS seria capaz de aumentar a confiança da previsão sem padrões. Padrões de aumentam a pontuação de confiança, sem ter de fornecer expressões com tantos.  

## <a name="pattern-matching"></a>Correspondência de padrões
Um padrão é correspondido com base em detetar as entidades dentro o padrão em primeiro lugar, em seguida, validar o resto das palavras e a ordem das palavras do padrão. Entidades são necessárias no padrão para um padrão corresponder. O padrão é aplicado ao nível do token, não no nível de caractere. 

## <a name="pattern-syntax"></a>Sintaxe de padrão
Sintaxe de padrão é um modelo para uma expressão. O modelo deve conter palavras e entidades que deseja correspondência com, bem como palavras e pontuação que pretende ignorar. É **não** uma expressão regular. 

Entidades em padrões são rodeadas por chavetas, `{}`. Padrões podem incluir entidades e entidades com funções. Pattern.any é uma entidade só é utilizada em padrões. A sintaxe é explicada nas seções a seguir.

### <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Sintaxe para adicionar uma entidade a um modelo padrão
Para adicionar uma entidade no modelo padrão, coloque o nome da entidade com chavetas, como `Who does {Employee} manage?`. 

|Padrão de entidade|
|--|
|`Who does {Employee} manage?`|

### <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Sintaxe para adicionar uma entidade e a função a um modelo padrão
Uma função de entidade é denotada como `{entity:role}` com o nome de entidade seguido por dois-pontos, em seguida, o nome da função. Para adicionar uma entidade com uma função no modelo padrão, coloque o nome da entidade e o nome de função com chavetas, como `Book a ticket from {Location:Origin} to {Location:Destination}`. 

|Padrão com funções de entidade|
|--|
|`Book a ticket from {Location:Origin} to {Location:Destination}`|

### <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Sintaxe para adicionar um pattern.any ao modelo de padrão
A entidade de Pattern.any permite-lhe adicionar uma entidade do comprimento diferentes para o padrão. O modelo padrão é seguido, desde que o pattern.any pode ser qualquer comprimento. 

Para adicionar um **Pattern.any** entidade no modelo padrão, coloque a entidade de Pattern.any com chavetas, como `How much does {Booktitle} cost and what format is it available in?`.  

|Padrão com Pattern.any entidade|
|--|
|`How much does {Booktitle} cost and what format is it available in?`|

|Títulos de livros no padrão|
|--|
|Quanto **roubar esse livro** custo e que formato está disponível em?|
|Quanto **perguntar** custo e que formato está disponível em?|
|Quanto **o incidente curioso de Dog no tempo de noite** custo e que formato está disponível em?| 

Nestes exemplos de título do livro, as palavras contextuais do título do livro não são confusas para LUIS. LUIS sabe onde o título do livro termina porque é um padrão e marcado com uma entidade de Pattern.any.

### <a name="explicit-lists"></a>Listas explícitas
Se o seu padrão contém um Pattern.any e permite que a sintaxe padrão para a possibilidade de uma extração de entidades incorreto com base na expressão, crie uma [lista explícita](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) por meio da API de criação para permitir que a exceção. 

Por exemplo, suponha que tenha um padrão que contém ambas as sintaxes opcional, `[]`e a sintaxe de entidade, `{}`, combinados numa forma de extrair dados incorretamente.

Considere o padrão '[find] e-mail sobre {subject} [de {pessoa}]'. Em expressões de com os seguintes, o **assunto** e **pessoa** entidade são extraídos corretamente e incorretamente:

|Expressão|Entidade|Extração correta|
|--|--|:--:|
|enviar um e-mail sobre cães Chris|assunto = cães<br>pessoa = Chris|✔|
|enviar um e-mail sobre o homem de La Mancha|assunto = o homem<br>pessoa = La Mancha|X|

Na tabela anterior, a expressão `email about the man from La Mancha`, o assunto deve ser `the man from La Mancha` (um título do livro), mas porque o requerente inclui a palavra opcional `from`, o título está previsto incorretamente. 

Para corrigir essa exceção para o padrão, adicione `the man from la mancha` como uma correspondência de lista explícita para a entidade de {subject} utilizando o [API de criação para lista explícita](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8).

### <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Sintaxe para marcar texto opcional numa expressão de modelo
Marcar o texto opcional na expressão usando a sintaxe de colchete de expressão regular, `[]`. O texto opcional pode aninhar Parênteses Retos até dois Retos apenas.

|Padrão com o texto opcional|
|--|
|`[find] email about {subject} [from {person}]`|

Marcas de pontuação, tal como `.`, `!`, e `?` podem ser ignorados com Parênteses Retos. Para poder ignorar essas marcas, cada marca deve estar num padrão separado. A sintaxe opcional atualmente não suporta a ignorar a um item numa lista de vários itens.

## <a name="patterns-only"></a>Padrões de apenas
LUIS permite que uma aplicação sem quaisquer expressões de exemplo na intenção. Esta utilização é permitida apenas se os padrões são utilizados. Padrões exigem pelo menos uma entidade em cada padrão. Para uma aplicação só por padrão, o padrão não deve conter entidades aprendidas por máquina porque estes requerem a expressões de exemplo. 

## <a name="best-practices"></a>Melhores práticas
Saiba mais [melhores práticas](luis-concept-best-practices.md).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba como implementar padrões neste tutorial](luis-tutorial-pattern.md)