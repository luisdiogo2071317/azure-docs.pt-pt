---
title: Identificação de parte da voz - API de análise linguística
description: Saiba como a etiquetagem de parte da voz na API de análise linguística identifica a categoria ou parte de voz de cada palavra de texto.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 09/27/2016
ms.author: lesun
ms.openlocfilehash: 636b8bc4ef41674d1e9ca12111aecee05870f381
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124573"
---
# <a name="part-of-speech-tagging"></a>Identificação de parte da voz

## <a name="background-and-motivation"></a>Em segundo plano e motivação

Assim que uma mensagem de texto foi separada em frases e tokens, a próxima etapa de análise é identificar a categoria ou parte da voz de cada palavra.
Estes incluem categorias, como *substantivo* (etc. em geral, que representa as pessoas, lugares, coisas, idéias,) e *verbo* (em geral, que representam ações, alterações de estado, etc. Para algumas palavras, a parte da voz é inequívoca (por exemplo, *quagmire* é realmente apenas um substantivo), mas para muitos outros, é difícil dizer.
*Tabela* poderia ser um lugar onde sentar-se (ou o esquema 2D números), mas pode também "tabela uma discussão sobre".

## <a name="list-of-part-of-speech-tags"></a>Lista de etiquetas de parte da voz

| Etiqueta | Descrição | Palavras de exemplo |
|-----|-------------|---------------|
| $ | Dólar | $ |
| \`\` | aspas de abertura | \` \`\` |
| '' | fechar as aspas | ' '' |
| ( | Parêntese de abertura | ( [ { |
| ) | parêntesis de fecho | ) ] } |
| ,  | por vírgulas | ,  |
| -- | Traço | -- |
| . | terminador de frase | . ! ? |
| : | dois pontos ou o botão de reticências | : ; ... |
| Cc | conjunto, coordenação | e mas ou ainda|
| CD | romano, cardinal | nove 20 1980 "96 |
| DT | determiner |um a um todos os dois nenhum|
| POR EXEMPLO | existencial daí | daí |
| FIREWALL | word externa | enfant terrible hoi polloi je ne sais quoi |
| ÍNDIA | preposition ou subordinating conjunto| no interior se após se |
| JJ | adjetivo ou romano, ordinal | nono multimodal bastante execrável |
| JJR | adjetivo, comparativa | melhor com mais rapidez mais barato |
| JJS | adjetivo, importantes | melhor mais rápida mais barato | 
| LS | marcador do item de lista | (a) (b) 1 2 A B B. a. |
| MD | auxiliar modal | Pode pode deverá será poderia poderá deve deve |
| NN | nome próprio, comum, singular ou em massa | Calçado de dinheiro Potato |
| NNP | nome próprio, adequada, singular | Kennedy Roosevelt Chicago Weehauken |
| NNPS | plural adequada, do substantivo, | Springfields Bushes |
| NNS | plural comuns, do substantivo, | campos de ratos de partes |
| PDT | pré-determiner | todos os dois metade muitos bastante tais-se de que isso |
| PEDIDOS DE COMPRA | Marcador genitive | "do |
| PRP | pronoun, pessoal | ela ele-podemos |
| $ PRP | pronoun, possessive | sua cópia seu respetivo meu nosso seus sua |
| RB | adverb | clinically única |
| RBR | adverb, comparativa | ainda mais gloomier grander graver maior grimmer mais difícil harsher mais segura mais pesada superior entretanto maior posterior mais enxutos mais demorado menos perfeitamente menor lonelier mais DB mais alto inferior mais... |
| RBS | adverb, importantes | melhor maiores bluntest mais antigo mais distante primeiro furthest mais difícil heartiest mais alto maior menos menos maioria mais próxima do segundo nesse pior |
| RP | Nuvem de partículas | desativado no limite de segurança sobre |
| SIMBÓLICO | Símbolo | % & |
| PARA | "para" como marcador preposition ou infinitive | para |
| OH | Interjection | OH felicidade howdy hello |
| REFERÊNCIA DO VB | verbo, o formulário básico | daremos a atribuir dinamicamente |
| VBD | verbo, tempo verbal do passado | deu atribuído flew |
| VBG | verbo, presente participle ou gerund | dando voar atribuição |
| VBN | verbo, participle passado | Tendo em conta atribuída voou |
| VBP | verbo, o tempo verbal presente, não 3º pessoa singular | daremos a atribuir dinamicamente |
| VBZ | verbo, o tempo verbal presente, 3ª pessoa singular | dá atribui flies |
| WDT | Q-determiner | que o que |
| WP | Q-pronoun | que quem |
| $ DO WP | Q-pronoun possessive | cujo |
| WRB | Q-adverb | como no entanto sempre que onde |

## <a name="specification"></a>Especificação

Como para atomização, Contamos com a especificação do [Penn Treebank](https://catalog.ldc.upenn.edu/ldc99t42).
