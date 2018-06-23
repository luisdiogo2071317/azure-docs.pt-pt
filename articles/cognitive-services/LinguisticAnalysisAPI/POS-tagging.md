---
title: Parte de voz etiquetagem na análise linguístico API | Microsoft Docs
description: Saiba como parte de voz etiquetagem nos serviços cognitivos Microsoft identifica a categoria ou a parte de voz de cada palavra de texto.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 09/27/2016
ms.author: lesun
ms.openlocfilehash: 35d2042db5f8d64c7dbd1df6bfdebcba6d6aab28
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351799"
---
# <a name="part-of-speech-tagging"></a>A marcação de parte de voz

## <a name="background-and-motivation"></a>Em segundo plano e a motivação

Depois de um texto foi separado para frases e tokens, o próximo passo do Analysis Services é identificar a categoria ou a parte de voz de cada palavra.
Estes incluem categorias, como *substantivo* (etc., geralmente, que representa as pessoas, locais, coisas, ideias,) e *verbo* (geralmente, que representa as ações, alterações de estado, etc. Para algumas palavras, a parte de voz é inequívoca (por exemplo, *quagmire* realmente é apenas um substantivo), mas para muitos outros, é difícil dizer.
*Tabela* pode ser um local onde a manter-se (ou o esquema 2-D números), mas pode também "tabela um debate".

## <a name="list-of-part-of-speech-tags"></a>Lista de etiquetas de parte de voz

| Etiqueta | Descrição | Palavras de exemplo |
|-----|-------------|---------------|
| $ | Dólar | $ |
| \`\` | aspas de abertura | \` \`\` |
| '' | aspas de fecho | ' '' |
| ( | Parêntese | ( [ { |
| ) | parênteses de fecho | ) ] } |
| , | vírgula | , |
| -- | Traço | -- |
| . | terminador de frases | . ! ? |
| : | vírgula ou por reticências | : ; ... |
| Cc | conjunto, coordenação | e mas ou ainda|
| CD | numeral, cardinal | nove 20 1980 ' 96 |
| TIPO DE IMPLEMENTAÇÃO | determiner |um a um todas as ambas nenhum|
| EX | daí existencial | não existe |
| FW | word externa | enfant terrible hoi polloi je ne sais quoi |
| EM | preposition ou subordinating conjunto| no interior se depois de se |
| JJ | adjective ou numeral, ordinal | ninth multimodal pretty execrable |
| JJR | adjective, comparative | melhor mais rapidamente mais barata |
| JJS | adjective, superlative | melhor fastest cheapest | 
| LS | marcador do item de lista | (a) (b) 1 2 A B B. a. |
| MD | modal auxiliar | Pode pode deverá será foi poderá deve aparece |
| NN | substantivo, comuns, único ou em massa | shoe de dinheiro Potato |
| NNP | substantivo, adequada, único | Kennedy Roosevelt Chicago Weehauken |
| NNPS | substantivo, adequada, plural | Springfields Bushes |
| NNS | substantivo, comuns, plural | campos de ratos de partes |
| PDT | pré-determiner | todos os ambas meio muitos bastante essas se isto |
| POS | Marcador genitive | ' do |
| PRP | pronoun, pessoal | ela ele-I iremos estes |
| PRP$ | pronoun, possessive | hers à minha nossa os respetivos sua |
| RB | adverb | clinically apenas |
| RBR | adverb, comparative | ainda mais mais gloomier grander graver maior grimmer mais difícil harsher healthier mais pesada superior no entanto a maior posterior leaner lengthier menos perfeitamente menor lonelier mais louder inferior... |
| RBS | adverb, superlative | melhor maiores bluntest mais antigo farthest primeiro furthest hardest heartiest máximo maior menos menos maioria mais próximo pior segundo tightest |
| RP | partícula | desativado no configurar Escalamento sobre |
| SYM | símbolo | % & |
| PARA | "a" como marcador preposition ou infinitive | para |
| SE | interjection | Se hooray howdy Olá |
| VB | verbo, formulário de base | Atribua o momento de atribuição |
| VBD | verbo, tense passado | deu atribuído flew |
| VBG | verbo, presente participle ou gerund | dar flying atribuição |
| VBN | verbo, participle passado | dada atribuído flown |
| VBP | verbo, tense presente, não 3rd pessoa único | Atribua o momento de atribuição |
| VBZ | Verbo tense presente, a pessoa 3rd único | Fornece atribui flies |
| WDT | Q determiner | que os que |
| WP | Q pronoun | quem quem |
| WP$ | Q-pronoun possessive | cujo |
| WRB | Q adverb | como no entanto sempre que onde |

## <a name="specification"></a>especificação

Para atomização, precisamos de especificação do [Penn Treebank](https://www.cis.upenn.edu/~treebank/).
