---
title: Saiba como padrões de aumentam a exatidão da previsão | Microsoft Docs
titleSuffix: Azure
description: Saiba como padrões para aumentar pontuações de predição intenção e localizar as entidades de conceção.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr
ms.openlocfilehash: 7ccc8353c91c354c745d52f9fc9663131f2b1c4e
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35356447"
---
# <a name="patterns-improve-prediction-accuracy"></a>Padrões de melhorar a exatidão da previsão
Padrões foram concebidos para melhorar a precisão quando várias utterances são muito semelhantes. Ao fornecer um padrão para o utterance, LUIS podem ter uma confiança elevada a predição. 

## <a name="patterns-solve-low-intent-confidence"></a>Padrões de resolverem confiança intenção baixa
Considere uma aplicação de recursos humanos relatórios no gráfico organizacional em relação a um empregado. Fornecido nome e a relação de um empregado, LUIS devolve os empregados envolvidos. Considere um empregado, personalizada, com um Gestor de Alice de nome e uma equipa de subordinados com o nome: Miguel, Rebecca e Carl.

![Imagem do organograma](./media/luis-concept-patterns/org-chart.png)

|Expressões|Objetivo de prever|Pontuação intenção|
|--|--|--|
|Quem é o subordinado de personalizada?|GetOrgChart|.30|
|Quem é o subordinado personalizada?|GetOrgChart|.30|

Se tiver uma aplicação entre 10 e 20 utterances com diferentes comprimentos de frase, ordem diferente do word e palavras mesmo diferentes (sinónimos de "subordinada", "Gerir", "relatório"), LUIS pode devolver uma pontuação baixa confiança. Para ajudar a LUIS compreender a importância da ordem word, crie um padrão. 

Padrões de resolverem as seguintes situações: 

* Quando o modelo de pontuação intenção for baixo
* Se a intenção correta não é a classificação superior, mas demasiado prestes a atingir a classificação superior. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Padrões não são uma garantia de intenção de
Padrões de utilizam uma combinação de tecnologias de predição. Definir um objetivo para um utterance modelo num padrão não é uma garantia da predição de intenção, mas é um sinal de forte. 

## <a name="patterns-do-not-improve-entity-detection"></a>Padrões não melhorar a deteção de entidade
Enquanto padrões necessitarem de entidades, um padrão não ajuda a detetar a entidade. Um padrão destina-se apenas para o ajudar a predição com pendentes e as funções.  

## <a name="prediction-scores-with-and-without-patterns"></a>Pontuações de predição com e sem padrões
Tendo em conta suficiente utterances de exemplo, LUIS conseguirão aumentar a confiança de predição sem padrões. Padrões de aumentam a classificação de confiança, sem ter de fornecer utterances tantos.  

## <a name="pattern-matching"></a>Correspondência de padrões
Um padrão é correspondido com base em detetar as entidades dentro padrão pela primeira vez, em seguida, validar o resto das palavras e ordem de word do padrão. Entidades são necessários o padrão para um padrão corresponder. 

## <a name="pattern-syntax"></a>Sintaxe de padrão
As entidades de padrões são rodeadas por chavetas. Padrões podem incluir entidades e entidades com funções. Pattern.any é uma entidade apenas utilizada em padrões. A sintaxe para cada um destes é explicada nas secções seguintes.

### <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Sintaxe para adicionar uma entidade a um modelo padrão
Para adicionar uma entidade para o modelo de padrão, coloque o nome da entidade com chavetas, tais como `Who does {Employee} manage?`. 

```
Who does {Employee} manage?
```

### <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Sintaxe para adicionar uma entidade e a função a um modelo padrão
Uma função de entidade está em falta que como `{entity:role}` com o nome da entidade seguido por um vírgula, em seguida, o nome da função. Para adicionar uma entidade com uma função para o modelo de padrão, coloque o nome da entidade e o nome de função com chavetas, tais como `Book a ticket from {Location:Origin} to {Location:Destination}`. 

```
Book a ticket from {Location:Origin} to {Location:Destination}
```

### <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Sintaxe para adicionar um pattern.any ao modelo de padrão
A entidade de Pattern.any permite-lhe adicionar uma entidade do comprimento diferentes para o padrão. Desde que o modelo de padrão é seguido, o pattern.any pode ser qualquer comprimento. 

Para adicionar um **Pattern.any** entidade para o modelo de padrão, coloque a entidade de Pattern.any com chavetas, tais como `How much does {Booktitle} cost and what format is it available in?`.  

```
How much does {Booktitle} cost and what format is it available in?
```

|Títulos de livro o padrão|
|--|
|Quanto **roubar este livro** custo e o formato está disponível?|
|Quanto **peça** custo e o formato está disponível?|
|Quanto **o incidente curiosidade de preguiçoso no tempo-noite** custo e o formato está disponível?| 

Nestes exemplos de título do livro, as nível contextual das palavras do título livro não são confusas para LUIS. LUIS sabe onde termina o título do livro porque está num padrão e marcados com uma entidade Pattern.any.

### <a name="explicit-lists"></a>Apresenta uma lista explícita
Se o padrão contém um Pattern.any e permite que a sintaxe de padrão para a possibilidade de extração uma entidade incorreto com base no utterance, crie um [lista explícita](https://aka.ms/ExplicitList) através da API de criação para permitir a exceção. 

Por exemplo, suponha que tem um padrão que contenha ambas sintaxe opcional, `[]`e a sintaxe de entidade, `{}`, combinados de forma para extrair dados incorretamente.

Considere o padrão '[encontrar] e-mail sobre {assunto} [{pessoa}]'. No utterances seguintes, o **requerente** e **pessoa** entidade são extraídos corretamente e incorretamente:

|Utterance|Entidade|Extração correta|
|--|--|:--:|
|e-mail sobre dogs a partir de Chris|requerente = dogs<br>pessoa = Chris|✔|
|e-mail sobre man do La Mancha|requerente = de man<br>pessoa = La Mancha|X|

Na tabela anterior, o utterance `email about the man from La Mancha`, o assunto deve ser `the man from La Mancha` (um título de livro) mas porque o requerente inclui o word opcional `from`, o título está incorretamente prever. 

Para corrigir esta exceção o padrão, adicionar `the man from la mancha` como uma correspondência da lista explícita para a entidade de {requerente} utilizando o [API de criação de lista explícita](https://aka.ms/ExplicitList).

### <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Sintaxe para marcar o texto opcional utterance um modelo
Marcar opcional texto utterance utilizando a sintaxe de parêntesis Reto de expressão regular `[]`. O texto opcional pode aninhar Parênteses Retos até duas Retos apenas.

```
[find] email about {subject} [from {person}]
```

Sinais de pontuação, tais como `.`, `!`, e `?` podem ser ignoradas utilizando parênteses Retos. Para ignorar estes marcas de escala, cada marca tem de estar num padrão separado. A sintaxe opcional atualmente não suporta a ignorar um item numa lista de vários itens.

## <a name="patterns-only"></a>Apenas padrões
LUIS permite que uma aplicação sem qualquer utterances de exemplo na intenção. Isto só é permitido se forem utilizados padrões. Padrões necessitam de pelo menos uma entidade em cada padrão. Para uma aplicação só de padrão, o padrão não deve conter entidades adquiridos por máquina porque esses requisitos requerem utterances de exemplo. 

## <a name="best-practices"></a>Melhores práticas
Saiba [melhores práticas](luis-concept-best-practices.md).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba como implementar padrões neste tutorial](luis-tutorial-pattern.md)

[LUIS]: luis-reference-regions.md
