---
title: Frases e tokens na API Analysis linguístico | Microsoft Docs
description: Saiba mais sobre a separação de frases e atomização na API Analysis linguístico em serviços cognitivos.
services: cognitive-services
author: DavidLiCIG
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 03/21/2016
ms.author: davl
ms.openlocfilehash: 4681098a0e56640e95463272be44f7432be26839
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351643"
---
# <a name="sentence-separation-and-tokenization"></a>Separação de frases e atomização

## <a name="background-and-motivation"></a>Em segundo plano e a motivação

Tendo em conta um corpo de texto, o primeiro passo do Analysis Services linguístico é de quebrar frases e tokens.

### <a name="sentence-separation"></a>Separação de frases

No primeiro rápida, parece que interrompendo texto em frases é simple: apenas localizar os marcadores de fim de frases e quebra de frases não existe.
No entanto, estas marcas são muitas vezes complicado e ambígua.

Considere o seguinte texto de exemplo:

> O que diga?!? Posso não ouvir sobre "novo licitação." o diretório É importante para Mr. e Santos Sra.

Este texto contém três frases:

- O que diga?!?
- Posso não ouvir sobre "novo licitação." o diretório
- É importante para Mr. e Santos Sra.

Tenha em atenção a como as extremidades de frases estão marcadas formas muito diferentes.
O primeiro termina numa combinação das marcas de pergunta e dos pontos de exclamação (por vezes denominados uma interrobang).
O segundo terminar com um período ou parar completo, mas as aspas seguintes deve ser solicitados para o frase anterior.
O frase terceiro, pode ver como esse mesmo caráter período pode ser utilizado para marcar abreviaturas bem.
Observar apenas pontuação fornece um conjunto de bom candidato, mas o trabalho adicional é necessário para identificar os limites de frases verdadeiro.

### <a name="tokenization"></a>Atomização

A próxima tarefa consiste interromper estas frases tokens.
A maioria das vezes, os tokens inglês são delimitados por espaços em branco.
(Localizar tokens ou palavras é muito mais fácil em inglês que no chinês, onde os espaços são utilizados principalmente não entre as palavras.
O frase primeiro pode ser escrito como "Whatdidyousay?")

Existem alguns casos difícil.
Em primeiro lugar, pontuação frequentemente (mas não sempre) deve ser dividida na direção oposta ao mesmo envolvente contexto.
Segundo, inglês tem *contrações*, como "não" ou "é", em que as palavras foram comprimidas e abreviadas em partes mais pequenas. O objetivo de atomizador é interromper a sequência de carateres palavras.

Vamos voltar para os frases de exemplo do acima.
Agora vamos tiver colocado "ponto center" (&middot;) entre cada token distinto.

- O que &middot; foi &middot; &middot; diga &middot; ?!?
- Posso &middot; foi &middot; n't &middot; ouvir &middot; sobre &middot; o &middot; director &middot; do &middot; " &middot; novo &middot; proposta &middot; . &middot; "
- - &middot; Do &middot; importante &middot; para &middot; Mr. &middot; e &middot; Mrs. &middot; Santos &middot; .

Tenha em atenção de que forma a maioria dos tokens são palavras encontrará no dicionário (por exemplo, *importante*, *director*).
Outras pessoas consistem apenas de pontuação.
Por fim, existem mais invulgares tokens para representar contrações como *n't* para *não*, como o possessives *do*, etc. Este atomização nos permite lidar palavra *não* e o frase *não* de forma mais consistente, para a instância.

## <a name="specification"></a>especificação

É importante tomar decisões consistentes sobre o que é composta por uma frase e um token.
Precisamos de especificação do [Penn Treebank](https://www.cis.upenn.edu/~treebank/) (alguns detalhes adicionais estão disponíveis aqui: [https://www.cis.upenn.edu/~treebank/tokenization.html]).
