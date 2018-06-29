---
title: Constituency análise na API Analysis linguístico | Microsoft Docs
description: Saiba mais sobre como constituency análise, também conhecido como "frase ao analisar a estrutura," identifica expressões no texto.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 03/21/2016
ms.author: lesun
ms.openlocfilehash: bff5e587621e1278c260d555aec280a0f4c7c8a1
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082177"
---
# <a name="constituency-parsing"></a>Constituency análise

O objetivo de constituency análise (também conhecido como "frase estrutura análise") é identificar os expressões no texto.
Isto pode ser útil quando extrair informações a partir de texto.
Os clientes ser útil para localizar os nomes de funcionalidade ou expressões chaves a partir de um grande corpo de texto e para ver os modificadores e ações envolvente cada expressão deste tipo.

## <a name="phrases"></a>Expressões

Para um linguist um *frase* é mais do que apenas uma sequência de palavras.
Para ser uma expressão, um grupo de palavras tem uma combinação de reproduzir uma função específica no frase.
Que o grupo palavras pode ser movido em conjunto ou substituído como um todo, e o frase deve permanecer fluent e gramáticas.

Considere o frase

> Pretende localizar um automóvel híbrida nova com o Bluetooth.

Esta frase contém o frase substantivo: "um automóvel híbrida nova com Bluetooth".
Como é que podemos saber que se trata de uma expressão de?
Iremos possam reescrever o frase (um pouco poetically) movendo o frase que toda para a frente:

> Um automóvel híbrida nova com Bluetooth pretendo encontrar.

Ou, iremos foi substitua esse frase de acesso uma expressão de função e significado, como "um carro novo fancy" semelhantes:

> Pretende localizar um carro fancy de novo.

Se em vez disso, é selecionado com outro subconjunto de palavras, estas tarefas de substituição seriam levar para um ou ilegíveis frases.
Considere o que acontece quando é mover "encontrar uma nova" para a frente:

> Localize um novo que pretender automóvel híbrida com o Bluetooth.

É muito difícil leia e compreenda os resultados.

O objetivo de um analisador é localizar todas as expressões deste tipo.
Interestingly, idioma natural, as expressões tendem a ser aninhada com uma da outra.
Uma representação natural destas expressões é uma árvore, tais como o seguinte:

![Árvore](./Images/tree.png)

Esta árvore ramos marcado como "NP" estão expressões substantivo.
Existem várias essas expressões: *posso*, *um automóvel híbrida nova*, *Bluetooth*, e *um automóvel híbrida nova com Bluetooth*.

## <a name="phrase-types"></a>Tipos de expressão

| Etiqueta | Descrição | Exemplo |
|-------|-------------|---------|
|ADJP   | Expressão adjective | ", por isso, grosseiro" |
|ADVP   | Expressão adverb | "Limpar através da Internet" |
|CONJP  | Expressão de conjunto | ", bem como" |
|FRAG   | Fragmento, utilizado para entradas incompletas ou fragmentary | "Altamente recomendado..." |
|INTJ   | interjection | "Hooray" |
|LST    | Marcador de lista, incluindo pontuação | "#4)" |
|NAC    | Não A constituintes, utilizado para indicar o âmbito de uma expressão não constituent |  "e para um bom grau" em "a dar e para um bom lidar" |
|NP | Expressão de nome | "um pancake tasty potato" |
|NX | Utilizado dentro de determinados NPs complexas para marcar o cabeçalho| |
|PP | Expressão Prepositional| "no conjunto de" |
|PRN    | Parenthetical| "(denominadas, por isso,)" |
|PRT    | partícula| "out" em "ripped out" |
|QP | Quantidade frase (ou seja, complexa medidas/quantidade) dentro de uma expressão de nome| "em torno $75" |
|RRC    | Cláusula reduzida do caminho relativo.| "ainda não pode estar resolvido" em "muitos problemas ainda não pode estar resolvidos" |
|S  | Cláusula ou frases. | "Este é uma frase."
|SBAR   | Cláusula subordinado, muitas vezes, introduzida por um conjunto subordinating | "como posso esquerda" em "Posso comparados em torno como posso deixado."|
|SBARQ  | Pergunta direta introduzida por uma palavra-q ou - expressão | "O que era o ponto de?" |
|SINV   | Frase declarativa Inverted | "Em nenhum momento foram estes Conhecimento." (tenha em atenção como o assunto normal "," foi movidos para depois do verbo "foram") |
|SQ | Inverted Sim/não pergunta ou cláusula de principal de uma pergunta q | "Recebem o automóvel?" |
|UCP    | Ao contrário do frase coordenada| "pequeno e com erros" (tenha em atenção a como uma adjective e uma expressão de preposition são conjoined com "e")|
|VP | Expressão de verbo | "Ocorreu as woods" |
|WHADJP | Frase de acesso q adjective | "como uncomfortable" |
|WHADVP | Frase de acesso q adverb| "quando" |
|WHNP   | Frase de acesso q-nome| "que potato", "quanto soup"|
|WHPP   | Frase de acesso q prepositional| "no qual país/região"|
|X  | Desconhecido, incerto ou unbracketable.| primeiro "a" em "a... o soup" |


## <a name="specification"></a>especificação

Árvores aqui utilizam expressões de S partir o [Penn Treebank](https://catalog.ldc.upenn.edu/ldc99t42).
